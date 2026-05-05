# 項目開發流程規範 v2.2

> 版本：v2.2
> 更新日期：2026-04-30
> 狀態：正式發布

---

## 一、流程總覽

```
需求接收 → PRD 起草 → 審批 → 技術方案 → 審批 → 測試用例 → 審批 → 實現 → 自測驗證 → 交付
```

**關鍵原則：**
- 每個審批節點必須獲得通過才能繼續
- 測試用例先行（先定義完成標準，再動手實現）
- **開發完成後必須對照測試用例逐項 AI 自測（所有 TC 必須通過）**
- AI 自測驗證必須包含 Console 錯誤檢查
- 交付必須有工具驗證，不能只靠截圖
- **未通過 AI 自測的項目不得交付**

---

## 二、審批狀態定義

| 狀態標識 | 含義 |
|---------|------|
| 🔄 起草中 | 文檔正在編寫，尚未完成 |
| ⚠️ 待審批 | 文檔已完成，等待審批 |
| ✅ 通過 | 審批通過，可以繼續 |
| ❌ 否決 | 審批不通過，需修改後重新審批 |
| 🔄 進行中 | 正在執行中 |
| ✅ 完成 | 已完成交付 |

---

## 三、PRD（產品需求文檔）

### 3.1 文件命名

```
docs/PRD/v{major}.{minor}_{功能名稱}.md
```

**示例：**
- `v8.0_WATCHLIST_MERGE.md`
- `v9.0_FAVORITES_GROUP.md`
- `v9.1_STAR_TO_GROUP_SELECTION.md`

### 3.2 PRD 必備內容

| 章節 | 內容 |
|------|------|
| 審批狀態 | 記錄每個節點的審批狀態 |
| 功能概述 | 背景問題、重構目標 |
| 用戶場景 | 核心場景、用戶價值 |
| 詳細設計 | 架構變更、UI 變更、數據模型 |
| 驗收標準 | 可量化、可測試的標準 |
| 執行記錄 | 每個操作的時間、結果、Owner |

### 3.3 審批節點

| 審批節點 | 說明 |
|---------|------|
| PRD 審批 | 產品經理審批功能需求 |
| 技術方案審批 | 技術評審可行性 |
| 測試用例審批 | QA 審批測試覆蓋 |
| 交付驗收 | 最終交付確認 |

---

## 四、技術方案

### 4.1 文件命名

```
docs/TECH_SPEC/v{major}.{minor}_{功能名稱}.md
```

### 4.2 技術方案必備內容

| 章節 | 內容 |
|------|------|
| 現有架構分析 | 當前架構、數據模型 |
| 目標架構 | 目標架構、變更點 |
| 具體修改方案 | 每個文件的修改內容 |
| 數據遷移策略 | 如有數據變更 |
| 測試驗證 | flutter analyze、build 命令 |
| 執行計劃 | 步驟清單 |

---

## 五、測試用例

### 5.1 文件命名

```
docs/TEST/TEST_CASES_v{major}.{minor}_{功能名稱}.md
```

### 5.2 測試用例模板

```markdown
## {用例編號}: {用例標題}

| 字段 | 內容 |
|------|------|
| 用例編號 | TC-XXX |
| 功能模組 | {模組} |
| 用例標題 | {標題} |
| 優先級 | P0/P1/P2 |
| 測試方式 | 自動化/手動 |
| 前置條件 | {條件} |
| 測試步驟 | {步驟} |
| 預期結果 | {結果} |
| 斷言 | {斷言} |
```

### 5.3 測試覆蓋要求

| 類型 | 覆蓋場景 |
|------|---------|
| 單元測試 | 數據模型、Controller 邏輯 |
| Widget 測試 | UI 組件交互 |
| 集成測試 | 完整頁面流程 |
| 回歸測試 | 不破壞現有功能 |

### 5.4 AI 自測強制要求

> **⚠️ 開發完成後、交付前：必須對照測試用例逐項 AI 自測，未通過不得交付**

開發完成後對照測試用例清單，確保每個 TC 都有對應的實現：

```bash
# 自測報告模板（每個 TC 必須標註狀態）
| TC-001 | NFT 卡片點擊導航 | ✅ 已實現 | 截圖驗證 |
| TC-002 | 詳細頁載入狀態 | ✅ 已實現 | Console 確認 |
...
```

| 狀態 | 定義 |
|------|------|
| ✅ 已實現 | 功能已實現，通過自測 |
| ⚠️ 部分實現 | 功能部分實現，需說明 |
| ❌ 未實現 | 功能未實現，禁止交付 |

---

## 六、實現流程

### 6.1 標準化實現命令

```bash
# 1. flutter analyze
flutter analyze lib/

# 2. flutter test（可選，但回歸測試必須通過）
flutter test

# 3. flutter build
flutter build ios --simulator --no-codesign

# 4. Device Console 錯誤檢查（新增強制關卡）
xcrun simctl spawn "iPhone 16 Pro" log show --predicate 'subsystem == "Flutter"' --last 1m 2>&1 | grep -i error

# 5. 卸載重裝驗證
xcrun simctl uninstall "iPhone 16 Pro" com.aimeteo.stockMarketApp
xcrun simctl install "iPhone 16 Pro" build/ios/iphonesimulator/Runner.app
xcrun simctl launch "iPhone 16 Pro" com.aimeteo.stockMarketApp

# 6. 截圖輔助驗證
xcrun simctl io "iPhone 16 Pro" screenshot /tmp/verify.png
```

### 6.2 AI 自測驗證（交付前置條件）

> **🚫 未通過以下所有測試不得交付**

| 測試項 | 合格標準 | 用途 |
|--------|---------|------|
| flutter analyze | 0 issues | 靜態分析 |
| flutter build | Success | 編譯通過 |
| Console 錯誤檢查 | 0 errors | Runtime 錯誤檢測 |
| 測試用例逐項覆蓋 | 100% TC 已實現 | 功能完整性 |
| App 卸載重裝 | Success, PID 可見 | 實際可用性 |

### 6.3 交付前測試用例對照清單

每次交付必須在交付報告中包含：

```markdown
### 功能測試矩陣（對照 TEST_CASES）

| TC | 測試項目 | 自測結果 | 驗證方式 |
|----|---------|---------|---------|
| TC-001 | 卡片點擊導航 | ✅ 已實現 | Get.currentRoute 確認 |
| TC-002 | 載入狀態 | ✅ 已實現 | Console 確認 |
... | ... | ... | ... |

**結論**：所有 TC 已實現並通過自測，可以交付。
```

> **重要提醒：** 截圖只能顯示 UI 靜態狀態，無法發現 Runtime 錯誤。截圖正常 ≠ 功能正常。

---

## 七、SSOT 交付驗證

> SSOT = Single Source of Truth（單一事實來源）

### 7.1 強制驗證清單

每次交付必須執行：

```bash
# 1. Write tool → 文件寫入磁盤
# 2. 立即執行（不等用戶催）
ls -la {文件路徑}
wc -c {文件路徑}
head -3 {文件路徑}
```

### 7.2 防虛假交付模式

```
❌ 禁止：只說"已交付"但從未執行 ls
❌ 禁止：只說"正在寫入"但 ls 結果從未展示
❌ 禁止：截圖正常就聲稱功能正常
```

---

## 八、版本命名規範

| 版本格式 | 說明 | 示例 |
|---------|------|------|
| v{major}.0 | 大版本，功能重構 | v9.0 |
| v{major}.{minor} | 小版本，功能迭代 | v9.1 |

**版本遞增規則：**
- 同一 PRD 內的功能迭代 → minor +1
- 全新 PRD → major +1

---

## 九、文檔目錄結構

```
docs/
├── PRD/                    # 產品需求文檔
│   ├── v8.0_WATCHLIST_MERGE.md
│   ├── v9.0_FAVORITES_GROUP.md
│   └── v9.1_STAR_TO_GROUP_SELECTION.md
├── TECH_SPEC/              # 技術方案
│   ├── v8.0_WATCHLIST_MERGE.md
│   ├── v9.0_FAVORITES_GROUP.md
│   └── v9.1_STAR_TO_GROUP_SELECTION.md
├── TEST/                  # 測試用例
│   ├── TEST_CASES_v8_WATCHLIST_MERGE.md
│   ├── TEST_CASES_v9_FAVORITES_GROUP.md
│   └── TEST_CASES_v9.1_STAR_TO_GROUP_SELECTION.md
└── AI/
    └── FLOWS/             # 流程規範
        ├── development-flow.md
        ├── bug-fix-flow.md
        └── prd-workflow.md

test/                      # 自動化測試代碼
├── widget_test.dart
├── exchange_test.dart
├── search_page_test.dart
└── {feature}_test.dart
```

---

## 十、Owner

AI Agent (Claude Code) / 產品經理
