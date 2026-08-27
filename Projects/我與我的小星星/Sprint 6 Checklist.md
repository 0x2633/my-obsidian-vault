# Sprint 6 — 手動測試清單

## 範圍

FCM 推播通知、Badge Count、多環境建置、色彩系統遷移、測試覆蓋率

---

## 1. FCM 推播通知

### 1.1 任務送出通知（冒險者 → 守護者）

- [X] 冒險者長按送出任務後，守護者收到推播通知
- [X] 通知標題與內容包含任務名稱
- [X] App 在前景時，顯示 local notification（非靜默）
- [X] App 在背景 / 關閉時，顯示系統推播通知
- [X] 點擊通知後正確導向 App

### 1.2 任務核准通知（守護者 → 冒險者）

- [X] 守護者點擊「擊掌確認」後，冒險者收到推播通知
- [X] 通知內容包含獲得的星星數
- [X] 前景 / 背景 / 關閉三種狀態皆能收到

### 1.3 獎勵兌換通知（冒險者 → 守護者）

- [X] 冒險者申請兌換獎勵後，守護者收到推播通知
- [X] 通知內容包含獎品名稱與所需星星數

### 1.4 Invalid Token 清理

- [X] 使用者登出後重新登入，舊的 FCM token 不影響通知發送
- [X] 無效 token 發送失敗後自動從 Firestore 移除

---

## 2. Badge Count（角標計數）

### 2.1 角標遞增

- [X] 收到推播通知時，App icon 上的角標數字正確遞增
- [X] 多則通知連續到達時，角標數字逐一累加（非固定為 1）
- [X] 角標數字與 Firestore `unread_count` 欄位一致

### 2.2 角標清除

- [X] 開啟 App 後（從背景回前景），角標立即清除為 0
- [X] Firestore `unread_count` 同步重設為 0
- [X] iOS：`applicationDidBecomeActive` 時 native 端即時清除
- [X] 多裝置情境：在 A 裝置清除角標後，B 裝置的角標下次推播時會重新從 0 計算

### 2.3 邊界條件

- [X] App 首次安裝（無 `unread_count` 欄位），角標正常運作
- [X] 快速連續收到多則通知，角標數字不漏計

---

## 3. 多環境建置 (Dev / Prod)

### 3.1 iOS 建置

- [x] `--dart-define=ENV=dev` 建置後，Bundle ID 為 dev 版本 ✅ 2026-03-11
- [x] `--dart-define=ENV=prod` 建置後，Bundle ID 為 prod 版本 ✅ 2026-03-11
- [x] 正確載入對應的 `GoogleService-Info-{env}.plist` ✅ 2026-03-11
- [x] APNs 推播在 dev 環境正常運作 ✅ 2026-03-11
- [ ] APNs 推播在 prod 環境正常運作

### 3.2 Android 建置

- [ ] `--dart-define=ENV=dev` 建置後，applicationId 為 dev 版本
- [ ] `--dart-define=ENV=prod` 建置後，applicationId 為 prod 版本
- [ ] 正確載入對應的 `google-services-{env}.json`
- [ ] FCM 推播在 dev 環境正常運作

### 3.3 Dart 端環境切換

- [x] `firebase_options.dart` 根據 `ENV` 變數正確選擇 Firebase 設定 ✅ 2026-03-11
- [x] dev 環境連接 `my-little-star-dev` Firebase 專案 ✅ 2026-03-11
- [x] prod 環境連接 `my-little-start` Firebase 專案 ✅ 2026-03-11

---

## 4. 色彩系統遷移（Slate → Stone）

### 4.1 守護者介面

- [X] 守護者首頁背景色為 Stone-50 `#FAF9F6`（溫暖白，非純白）
- [X] 文字色為 Stone-800 `#292524`（溫暖深色，非純黑）
- [X] 卡片、按鈕等元件顏色與 Stone 色系一致
- [X] 無殘留的 Slate 色系（偏藍灰色調）

### 4.2 冒險者介面

- [X] 冒險者首頁色彩與 Stone 色系一致
- [X] 星星顯示為 Amber `#FBBF24`
- [X] 完成狀態為 Emerald `#10B981`
- [X] 錯誤 / 退回狀態為 Rose `#FB7185`（非純紅色）

### 4.3 深色 / 淺色模式

- [X] 淺色模式下色彩正確
- [X] 深色模式下色彩正確（如有支援）

---

## 5. Cloud Functions

### 5.1 onTaskSubmitted

- [X] 冒險者送出任務後，Cloud Function 正確觸發
- [X] 通知發送至該家庭所有守護者
- [X] Function 日誌無錯誤

### 5.2 onTaskApproved

- [X] 守護者核准任務後，Cloud Function 正確觸發
- [X] 通知發送至該冒險者
- [X] `unread_count` 正確遞增

### 5.3 onRedemptionCreated

- [X] 冒險者申請兌換後，Cloud Function 正確觸發
- [X] 通知發送至該家庭所有守護者

---

## 6. 其他 UI 改進

### 6.1 多冒險者任務建立

- [X] 多冒險者家庭時，建立任務可選擇指派對象
- [X] 單冒險者家庭時，冒險者選擇器自動隱藏

### 6.2 獎勵系統改進

- [X] 星星消費上限提升至 999
- [X] 可直接輸入星星數（非僅步進器）
- [X] 獎勵可指派給特定冒險者
- [X] 「All Adventurers」選項已移除

### 6.3 守護者首頁標題

- [X] 守護者首頁標題顯示家庭名稱

### 6.4 App 名稱

- [X] App 顯示名稱為「My Little Stars」（非 Family Quest App）

---

## 7. 前景通知（flutter_local_notifications）

### 7.1 Android

- [X] App 在前景時收到 FCM，顯示 high importance 通知
- [X] 通知包含正確的 channel name
- [X] 通知有聲音 / 震動

### 7.2 iOS

- [X] App 在前景時收到 FCM，顯示 banner 通知
- [X] 通知有聲音

---

## 測試環境

- [X] iOS 實機（推播必須用實機）
- [X] Android 實機（推播必須用實機）
- [X] Dev 環境測試通過
- [ ] Prod 環境測試通過
- [X] 三種語言環境皆測試通知文案
