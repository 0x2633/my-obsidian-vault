---
tags:
  - flutter
  - app
---


> **致 Code Agent 的系統提示 (System Prompt for Code Agents)**
> 
> 你現在是一位精通 Flutter 與 Firebase 的資深軟體架構師。你的任務是根據本文件開發一款名為「我與我的小星星」的家庭遊戲化正向教養 App。
> 
> 在執行任務時，你必須嚴格遵守以下 `<Agent_Rules>`，並根據 `<Execution_Plan>` 逐步產出程式碼。

## 0. 🤖 Agent 嚴格執行規範 `<Agent_Rules>`

### 0.1 核心架構規範 (Clean Architecture)

- **目錄結構**：嚴格遵守 `features/` (依功能分包) 的 Clean Architecture 結構。每個 feature 下必須包含 `data`, `domain`, `presentation` 三層。
    
- **依賴注入**：使用 `get_it` 與 `injectable` 進行依賴注入。禁止在 UI 層直接實例化 Repository 或 UseCase。
    
- **狀態管理**：全面使用 `flutter_bloc` (BLoC/Cubit)。
    
- **錯誤處理**：禁止使用原生的 `Exception` 傳遞至 UI。必須在 Data 層攔截 Firebase 錯誤，並轉換為 Domain 層自定義的 `Failure` 類別 (搭配 `fpdart` 或 `dartz` 的 `Either` 進行回傳)。
    

### 0.2 TDD 與 Git Commit 強制規範 (Critical)

基於本專案嚴格的工程紀律要求，你必須採用「紅-綠-重構 (Red-Green-Refactor)」的 TDD 流程。**在執行 `git commit` 時，強制使用以下帶有 Emoji 標籤的語意化提交格式**（此為專案核心規定，禁止省略 Emoji）：

- 🔴 **(Red)**: `🔴 test(scope): 描述` (提交一個失敗的測試，此時不得有實作代碼)。
    
- 🟢 **(Green)**: `🟢 feat(scope): 描述` (提交使測試通過的最小實作代碼)。
    
- ♻️ **(Refactor)**: `♻️ refactor(scope): 描述` (對剛通過的代碼進行優化，測試必須保持通過)。
    

### 0.3 測試工具棧

- 單元測試：領域層與資料層邏輯。
    
- Firestore 模擬：**絕對禁止** Mock `FirebaseFirestore` 類別，必須全面使用 `fake_cloud_firestore` 進行記憶體資料庫測試。
    
- 狀態測試：使用 `bloc_test` 驗證狀態流轉。
    

---

## 1. 📖 專案脈絡 (Project Context)

### 1.1 產品願景

「我與我的小星星」是一款符合 UI/UX Pro Max 標準的家庭教養 App。旨在將枯燥的家務轉化為「冒險任務」，透過積極心理學的正向強化（Positive Reinforcement），讓兒童在遊戲化體驗中獲得成就感。

### 1.2 雙角色體系

- **🛡️ 守護者 (家長)**：發布任務、審核任務（給予擊掌確認）、管理寶藏庫與星星兌換門檻。
    
- **🚀 冒險者 (兒童)**：透過長按按鈕完成任務、累積星星、點擊寶藏庫進行兌換。
    

### 1.3 UI/UX 與文案規範 (Positive UX Writing)

- **主色調**：琥珀星芒 `Amber-400 (#FBBF24)` (主視覺/星星)、翡翠綠 `Emerald-500 (#10B981)` (成功/確認)、珊瑚粉 `Rose-400 (#FB7185)` (退回/溫和除錯)。禁止使用純紅色警告。
    
- **文案風格**：正向、鼓勵。例如，網路錯誤顯示「通訊水晶暫時失去能量」，星星不足顯示「還差一點點魔法！」。
    

---

## 2. 🏗️ 資料庫架構與安全規則 `<Database_Schema>`

實作時請嚴格依照以下 JSON 結構建立 Models 與 Entities。

### 2.1 Firestore Collections

- **`families`**: `{ family_id, parent_uids: [string], subscription_tier: 'trial'|'free'|'pro', created_at }`
    
- **`users`**: `{ uid, family_id, role: 'guardian'|'adventurer', nickname, stars_balance: int, fcm_tokens: [string] }`
    
- **`task_blueprints`**: `{ blueprint_id, family_id, assignee_uid, title, stars_reward, is_daily_recurring: bool, is_active }`
    
- **`task_instances`**: `{ instance_id, family_id, blueprint_id, assignee_uid, date_string: 'YYYY-MM-DD', status: 'available'|'pending'|'completed' }`
    
- **`rewards`**: `{ reward_id, family_id, title, stars_cost, icon_id, is_active }`

- **`redemptions`**: `{ redemption_id, family_id, adventurer_uid, reward_id, reward_title, stars_cost, status: 'pending'|'approved'|'rejected', created_at }`

- **`transactions`**: `{ transaction_id, family_id, user_uid, type: 'earn'|'spend', amount, description, created_at }`
    

### 2.2 核心安全與防呆機制 (Implementation Requirements)

- **發放星星**：守護者審核通過時，必須使用 Firestore **`WriteBatch`** 同時更新 `task_instances.status` 與 `users.stars_balance` (`FieldValue.increment`)。
    
- **扣除星星**：冒險者兌換獎品時，必須使用 Firestore **`Transaction`** 先讀取 `users.stars_balance`，確認餘額 `>= stars_cost` 後，再執行扣款並寫入 `transactions` 表，嚴防超扣。
    
- **COPPA 刪除機制**：需實作 Firebase Cloud Function，當守護者點擊刪除冒險者時，遞迴清除上述表中的關聯 UID 資料。
    

---

## 3. ✅ 核心驗收標準 `<Acceptance_Criteria>`

Agent 在撰寫 🔴 Red 測試案例時，必須直接對應以下 AC：

- **UC 1 (綁定)**: 冒險者輸入 6 位數密碼後，系統必須核發 Firebase Anonymous Auth，並綁定對應的 `family_id`。家長端須透過 Stream 監聽狀態，配對成功自動關閉視窗。
    
- **UC 2 (任務)**: 冒險者送出任務後，狀態僅轉為 `pending`，**絕對不可提前增加星星餘額**。
    
- **UC 3 (兌換)**: 當星星餘額不足時，Repository 必須拋出 `InsufficientStarsFailure`；若充足，需確保 Transaction 成功執行並新增 `transactions` 紀錄。
    
- **UC 4 (試用期)**: 根據 `families.created_at` 判斷，若小於 14 天，無條件允許建立超過 3 個 `task_blueprints`。
    

---

## 4. 🚀 開發任務拆解與執行計畫 `<Execution_Plan>`

> **Agent 指令**：請依序讀取以下 Sprint 任務。每完成一個 Sub-task，請暫停並詢問使用者是否繼續，並確保每次代碼提交都嚴格遵守 🔴 🟢 ♻️ TDD 規範。

### Sprint 1: 基礎建設與設備安全綁定 (Feature: Auth & Pairing) ✅ 已完成

- **Task 1.1**: ✅ 初始化 Flutter 專案，設定 Clean Architecture 資料夾結構，並配置 `get_it`, `injectable`, `fpdart`。
- **Task 1.2**: ✅ 建立 `AuthRepository` 介面與 `User` Entity，實作 6 位數密碼登入邏輯。
- **Task 1.3**: ✅ 實作 `AuthRemoteDataSource`，使用 `FirebaseAuth.signInAnonymously()`。
- **Task 1.4**: ✅ 實作客製化「專屬數字鍵盤 Widget」，包含 `HapticFeedback` 觸覺回饋。
- **Task 1.5**: ✅ 實作家長端的 Firestore Stream 監聽邏輯，自動關閉配對對話框。

### Sprint 2: 任務引擎與延遲加載 (Feature: Tasks) ✅ 已完成

- **Task 2.1**: ✅ 建立 `TaskRepository` 介面，實作 Lazy Loading 邏輯。
- **Task 2.2**: ✅ 使用 `fake_cloud_firestore` 實作資料層與測試。
- **Task 2.3**: ✅ 實作「長按 2 秒 (Long-Press)」防呆送出 Widget，與 `TaskCubit` 串接。
- **Task 2.4**: ✅ 實作守護者審核邏輯，`WriteBatch` 正確將狀態改為 `completed` 並 `increment` 星星數。

### Sprint 3: 獎勵系統與防超扣機制 (Feature: Rewards) ✅ 已完成

- **Task 3.1**: ✅ 建立 `RewardRepository` 介面，實作「星星餘額不足」的失敗攔截。
- **Task 3.2**: ✅ 實作基於 Firestore `runTransaction` 的原子扣款邏輯與交易紀錄寫入。
- **Task 3.3**: ✅ 實作 BLoC 層與 UI，正向文案「還差一點點魔法！」。
- **Task 3.4**: ✅ 實作兌換審核流（`redemptions` 表），守護者批准/拒絕。

### Sprint 4: UI 打磨與使用者體驗 ✅ 已完成

- **Task 4.1**: ✅ Slidable 滑動操作（任務卡、獎勵卡：編輯與刪除）。
- **Task 4.2**: ✅ 任務編輯全端實作（WriteBatch 同步更新 blueprint + instance）。
- **Task 4.3**: ✅ 已完成任務禁用編輯（僅允許刪除）。
- **Task 4.4**: ✅ RPG 風格冒險者頁面（Lottie 動畫、星星動畫標頭）。
- **Task 4.5**: ✅ 任務與獎勵歷史週瀏覽功能。
- **Task 4.6**: ✅ FCM 推播通知（任務送審、星星發放、獎勵兌換）。
- **Task 4.7**: ✅ 修正設定頁 ListTile 邊框首次渲染跑版問題。

### Sprint 5: 國際化與多語系 🔄 進行中

- **Task 5.1**: ✅ 完成三語系翻譯（EN / ZH / ZH_TW）。
- **Task 5.2**: ✅ 守護者設定頁語言切換器（LocaleProvider + SharedPreferences）。
- **Task 5.3**: ✅ 狀態文字正向化（冒險進行中 / Quest in progress）。