# AI 開發閉環知識庫 v2.1

> 創建日期：2026-04-29
> 更新日期：2026-04-30
> 版本：v2.1
> 用途：跨項目通用的 AI Agent 開發閉環 SOP
> 特性：可直接複製 `docs/AI/` 目錄到任何新項目執行

---

## 快速開始

```
1. 將 docs/AI/ 複製到新項目
2. 將 docs/PRD/、docs/TECH/、docs/TEST/ 建立（模板見 TEMPLATES/）
3. 按 FLOWS/development-flow.md 執行全流程
```

---

## 目錄結構

```
docs/AI/                              # AI 知識庫（跨項目通用，可直接複製）
├── README.md                        # 本文件 - 知識庫總覽
├── SKILLS/                          # 技能速查
│   ├── pua-skills-index.md         # PUA skill 索引
│   ├── flutter-workflow.md          # Flutter 開發 workflow
│   └── ai-self-test-verification.md # AI 自測驗證 Skill
├── FLOWS/                           # 流程規範（通用）
│   ├── development-flow.md          # 開發閉環（含 AI 自測 + SSOT）
│   ├── bug-fix-flow.md            # Bug 修復閉環
│   ├── debug-flow.md              # Debug 根因分析流程
│   └── prd-workflow.md           # PRD 管理流程
└── TEMPLATES/                      # 文檔模板
    ├── prd-template.md            # PRD 文件模板
    └── tech-spec-template.md     # 技術方案模板

{項目根目錄}/                        # 項目具體文檔
├── docs/PRD/                        # 產品需求文檔
├── docs/TECH/                       # 技術方案
└── docs/TEST/                      # 測試用例
```

---

## 流程總覽

```
需求 → PRD 生成 → ⚠️ 用戶審批 → 技術方案 → ⚠️ 用戶審批 → 測試用例 → ⚠️ 用戶審批 → 實現 → AI 自測 → 交付
```

**強制關卡：**
- PRD 未通過審批 = 不能生成技術方案
- 技術方案未通過審批 = 不能開始實現
- **每次交付前必須執行 SSOT 驗證（ls + wc）**

---

## 核心流程

### 1. 開發閉環（development-flow.md）

| 步驟 | 動作 | 交付物 |
|------|------|--------|
| 接收需求 | 理解 + 問清楚模糊點 | 需求澄清 |
| 定目標 | 顆粒度拆解到可執行任務 | Task List |
| PRD 生成 | 寫入 `docs/PRD/` | PRD 文件 |
| 實現 | 代碼 + 測試 | 可編譯代碼 |
| AI 自測 | 對照 PRD 逐項驗證 | 自測報告 |
| SSOT 交付驗證 | ls + wc + Console 檢查 | 交付截圖 |
| 復盤 | 四步法 | SOP 沉澱 |

### 2. Bug 修復閉環（bug-fix-flow.md）

| 步驟 | 動作 |
|------|------|
| 接收 Bug | 症狀確認 + 複現步驟 |
| 根因分析 | 5-Why / 揪頭髮 |
| 修復方案 | 設計 + 評審 |
| 代碼修復 | Write tool + SSOT 驗證 |
| 自測驗證 | analyze + test + Console 檢查 |
| 同類問題掃描 | 藍軍自攻擊 |

### 3. PRD 管理流程（prd-workflow.md）

| 步驟 | 動作 |
|------|------|
| PRD 生成 | 寫入 `docs/PRD/vX.Y_NAME.md` |
| ⚠️ 用戶審批 | 方向對了再動手 |
| 技術方案 | 寫入 `docs/TECH/vX.Y_NAME.md` |
| ⚠️ 用戶審批 | 方案對了再動手 |
| 測試用例 | 寫入 `docs/TEST/TEST_CASES_vX.md` |
| ⚠️ 用戶審批 | 測試覆蓋足夠了再動手 |

---

## SSOT 交付驗證（防止虛假交付）

> **底層邏輯**：口說無憑，工具驗證才是事實。

**每次交付必須執行：**

```bash
# 1. 文檔交付
ls -la docs/PRD/vX.Y_NAME.md
wc -c docs/PRD/vX.Y_NAME.md

# 2. 代碼交付
flutter analyze && flutter test

# 3. Build 交付
flutter build ios --simulator --no-codesign

# 4. Console 錯誤檢查（新增強制關卡）
xcrun simctl spawn "iPhone 16 Pro" log show --predicate 'subsystem == "Flutter"' --last 1m 2>&1 | grep -i error
```

**合格標準：**
- `error` 出現次數 = 0 ✅
- `error` 出現次數 > 0 ❌ → 必須修復後才能交付

**反模式（禁止）：**
```
❌ 只說"已生成"
❌ 只說"已交付"
❌ 截圖正常就聲稱功能正常（截圖無法發現 Runtime 錯誤）
```

---

## 三條紅線（安全紅線）

| 紅線 | 內容 |
|------|------|
| 紅線一 | 閉環意識：聲稱完成前必須跑驗證命令貼輸出 |
| 紅線二 | 事實驅動：未驗證的歸因是甩鍋 |
| 紅線三 | 窮盡一切：未走完 5 步方法論禁止放棄 |

---

## Owner

AI Agent (Claude Code)
