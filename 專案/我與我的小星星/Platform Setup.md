# Platform Setup Guide

This document covers iOS, Android, and Firebase platform configurations for the My Little Stars project.

## Multi-Environment Architecture

The project uses two separate Firebase projects:

| Environment | Firebase Project ID | Bundle / Application ID |
| --- | --- | --- |
| dev | `my-little-star-dev` | `io.lzpreneur.mylittlestar.dev` |
| prod | `my-little-start` | `io.lzpreneur.mylittlestar` |

Environment is selected at build time via `--dart-define=ENV=dev|prod` (defaults to `dev`).

## iOS Setup

### Bundle ID Switching (xcconfig)

iOS bundle ID is controlled via xcconfig files, not hardcoded in `project.pbxproj`:

1. `build_ios.sh` writes the correct `PRODUCT_BUNDLE_IDENTIFIER` to `ios/Flutter/Env.xcconfig`
2. `Debug.xcconfig` and `Release.xcconfig` both `#include "Env.xcconfig"`
3. The Runner target in `project.pbxproj` does **not** set `PRODUCT_BUNDLE_IDENTIFIER` in its build settings — this allows the xcconfig value to take effect

**Important**: If Xcode re-adds `PRODUCT_BUNDLE_IDENTIFIER` to the pbxproj (e.g., via the UI), the xcconfig value will be overridden. Remove it from the pbxproj if this happens.

### GoogleService-Info.plist Switching

Two plist files are stored in `ios/Runner/`:

- `GoogleService-Info-dev.plist` — for dev Firebase project
- `GoogleService-Info-prod.plist` — for prod Firebase project

An Xcode build phase script (`ios/scripts/copy_google_service.sh`) reads the `ENV` value from `DART_DEFINES` and copies the correct plist into the app bundle at build time.

**Xcode Build Phase Setup**: Add a "Run Script" build phase in the Runner target (before "Copy Bundle Resources") with:

```bash
"${SRCROOT}/scripts/copy_google_service.sh"
```

### APNs (Push Notifications)

- **Auth Key (.p8)** is the recommended approach — it never expires and works for both sandbox and production APNs environments
- The same Auth Key can be uploaded to multiple Firebase projects
- In Firebase Console → Project Settings → Cloud Messaging:
  - Upload the Auth Key to **both** the "APNs Authentication Key (Development)" and "APNs Authentication Key (Production)" slots
  - This is required because:
    - Debug builds (Xcode, simulator) → sandbox APNs environment
    - Ad-hoc / App Store builds → production APNs environment

### iOS Build Command

Always use the wrapper script:

```bash
./build_ios.sh dev     # dev environment
./build_ios.sh prod    # prod environment
```

This handles: writing `Env.xcconfig`, running `flutter clean`, and building the IPA with the correct `--dart-define`.

## Android Setup

### Application ID Switching

`android/app/build.gradle.kts` handles environment switching:

1. Parses `dart-defines` from Gradle properties (base64-decoded) to extract `ENV`
2. Sets `applicationId` dynamically based on ENV
3. Namespace remains `io.lzpreneur.mylittlestar.dev` regardless of ENV

### google-services.json Switching

Two JSON files are stored in `android/app/`:

- `google-services-dev.json` — for dev Firebase project
- `google-services-prod.json` — for prod Firebase project

A custom Gradle task `copyGoogleServices` runs before Google Services processing and copies the correct file to `google-services.json`.

### Android-Specific Configurations

- **Core Library Desugaring**: Enabled (`coreLibraryDesugaring`) with `desugar_jdk_libs:2.1.4` for Java 8+ API support on older Android versions
- **Java/Kotlin Target**: Java 17
- **16 KB Page Size**: `jniLibs.useLegacyPackaging = false` for Android 15+ compatibility
- **Firebase BoM**: Version 34.8.0
- **Notification Channel**: `high_importance_channel` for heads-up notifications

### Android Build Command

```bash
flutter build apk --dart-define=ENV=dev
flutter build apk --dart-define=ENV=prod
```

## Firebase Cloud Functions

Located in `firebase/functions/src/`.

### Notification Functions

| Function | Trigger | Description |
| --- | --- | --- |
| `onTaskInstanceUpdate` | Firestore update on `task_instances/{id}` | Notifies guardian when task submitted; notifies adventurer when stars awarded |
| `onRedemptionWrite` | Firestore write on `redemptions/{id}` | Notifies guardian when reward redeemed; notifies adventurer on approval/rejection |

### Send Helpers (`send-helpers.ts`)

- `getGuardianTokens(familyId)` — Looks up all guardian FCM tokens for a family
- `getUserToken(uid)` — Looks up a specific user's FCM token
- `sendToTokens(tokens, notification, data?)` — Sends per-user FCM messages with:
  - Atomic `unread_count` increment per recipient (for accurate iOS badge)
  - Android high-priority with `high_importance_channel`
  - iOS badge count and default sound
  - Automatic cleanup of invalid/expired FCM tokens

### Deploying Functions

```bash
cd firebase && firebase deploy --only functions
```

Deploy to dev project by default. For prod, use `firebase use prod` first or specify `--project my-little-start`.

## Badge Count System

### Architecture Overview

Badge count is managed through a **server-side counter + client-side clearing** pattern:

```text
[Cloud Function] ─── increment unread_count ──→ [Firestore /users/{uid}]
       │                                              │
       │ send FCM with badge=N                        │ read current count
       ▼                                              ▼
[APNs/FCM] ──→ [iOS badge on app icon]    [clearBadge() on app open]
```

### Server Side (Cloud Functions)

**File**: `firebase/functions/src/notifications/send-helpers.ts`

`sendToTokens()` processes each token in parallel via `Promise.all`:

1. Query user by `fcm_token` in Firestore
2. Atomically increment `unread_count` using `db.runTransaction()` — prevents race conditions when multiple notifications arrive simultaneously
3. Read back the new count and set it as the `badge` value in the APNs payload
4. Send per-user FCM messages via `admin.messaging().sendEach()`

**Why transaction instead of `FieldValue.increment()`**: With `FieldValue.increment()`, the server cannot read the updated value in the same operation. A separate `get()` after `update()` creates a race condition where concurrent notifications may read stale values. The transaction guarantees atomic read-increment-write.

### Client Side — iOS Badge Clearing (Two Layers)

Badge clearing uses a **two-layer approach** to handle timing issues between native iOS lifecycle and Flutter engine:

#### Layer 1: Native `applicationDidBecomeActive` (earliest possible moment)

**File**: `ios/Runner/AppDelegate.swift`

```swift
override func applicationDidBecomeActive(_ application: UIApplication) {
    super.applicationDidBecomeActive(application)
    clearNativeBadge()  // removeAllDeliveredNotifications + setBadgeCount(0)
}
```

This runs **before** the Flutter engine processes messages, preventing Firebase SDK from re-applying badge values from pending APNs payloads.

#### Layer 2: Flutter `didChangeAppLifecycleState` (Firestore reset)

**File**: `lib/main.dart` → `lib/features/notifications/data/services/notification_service.dart`

```dart
void didChangeAppLifecycleState(AppLifecycleState state) {
    if (state == AppLifecycleState.resumed) {
        getIt<NotificationService>().clearBadge();  // resets Firestore unread_count
    }
}
```

`clearBadge()` performs three operations:

1. `_localNotifications.cancelAll()` — clears flutter_local_notifications
2. `_badgeChannel.invokeMethod('clearBadge')` — native badge clearing via MethodChannel
3. Firestore `unread_count` reset to 0

### MethodChannel Registration

**Important**: The `FlutterMethodChannel` must be registered via `FlutterPluginRegistry` API in `didInitializeImplicitFlutterEngine`, **not** by accessing `window.rootViewController` in `didFinishLaunchingWithOptions`. The latter is deprecated and returns `nil` in newer Flutter versions, causing the channel handler to never be registered.

```swift
func didInitializeImplicitFlutterEngine(_ engineBridge: FlutterImplicitEngineBridge) {
    guard let registrar = engineBridge.pluginRegistry.registrar(forPlugin: "BadgePlugin") else { return }
    badgeChannel = FlutterMethodChannel(name: "...", binaryMessenger: registrar.messenger())
}
```

The channel must be stored as an **instance property** (`private var badgeChannel`) to prevent ARC from deallocating it.

### Troubleshooting

| Symptom | Cause | Fix |
| --- | --- | --- |
| Badge always 1, never increments | Cloud Functions not redeployed after code change | `cd firebase && firebase deploy --only functions` |
| Badge doesn't clear on app open | MethodChannel registered in deprecated `didFinishLaunchingWithOptions` | Move to `didInitializeImplicitFlutterEngine` with `FlutterPluginRegistry` |
| Badge briefly disappears then reappears | `clearBadge` runs too late (after Firebase SDK re-applies badge) | Add `applicationDidBecomeActive` native clearing |
| Badge shows on app icon but `unread_count` is 0 | Firestore cleared but iOS native badge not cleared | Ensure `removeAllDeliveredNotifications()` + `setBadgeCount(0)` both run |
| `MissingPluginException` silently caught | Channel not registered (ARC deallocated or nil registrar) | Store channel as instance property; use `guard let` for registrar |
