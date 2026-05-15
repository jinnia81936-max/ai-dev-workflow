# AI 開發閉環工作流

> 跨項目通用的 AI Agent 開發閉環 SOP，涵蓋新功能開發與 Bug 修復兩條流程，嚴格遵守審批門禁。

---

## 流程紅線（不可違反）

### 新功能開發流程

```
需求 → PRD 生成 → ⚠️ 用戶審批 → 技術方案 → ⚠️ 用戶審批 → 測試用例 → ⚠️ 用戶審批 → 實現 → AI 自測 → 交付
```

### Bug 修復流程

```
問題 → bug分析報告 → ⚠️ 用戶審批 → bug修復技術方案 → ⚠️ 用戶審批 → 測試用例 → ⚠️ 用戶審批 → 實現 → AI 自測 → 交付
```

**三條共通紅線：**
1. 每個階段必須用戶審批通過後才能進入下一階段，禁止跳過
2. 聲稱交付前必須執行 SSOT 驗證（Write → ls → wc -c → Read）
3. 命令輸出才是證據，截圖不等於證據

---

## 一鍵安裝

```bash
git clone https://github.com/jinnia81936-max/ai-dev-workflow.git /tmp/ai-dev-workflow && \
mkdir -p ~/.agents/skills/ai-dev-workflow ~/.agents/skills/bug-fix-workflow && \
cp /tmp/ai-dev-workflow/SKILLS/ai-dev-workflow/SKILL.md ~/.agents/skills/ai-dev-workflow/SKILL.md && \
cp /tmp/ai-dev-workflow/SKILLS/bug-fix-workflow/SKILL.md ~/.agents/skills/bug-fix-workflow/SKILL.md && \
rm -rf /tmp/ai-dev-workflow && \
echo "✅ AI 開發閉環工作流安裝完成（新功能開發 + Bug 修復）"
```

---

## 快速開始（新項目）

```bash
# 1. 安裝技能到本地（執行上方一鍵安裝命令）

# 2. 在新項目中建立文檔目錄
mkdir -p docs/PRD docs/TECH docs/TEST docs/BUG

# 3. 在 AI Agent 中提出需求或 Bug，Agent 會按對應流程逐步執行並請求審批
```

---

## 目錄結構

```
~/.agents/skills/
├── ai-dev-workflow/
│   └── SKILL.md              # 新功能開發技能
└── bug-fix-workflow/
    └── SKILL.md              # Bug 修復技能

{項目根目錄}/
├── docs/
│   ├── PRD/                  # 產品需求文檔
│   ├── TECH/                 # 技術方案文檔（新功能 + Bug 修復）
│   ├── TEST/                 # 測試用例文檔（新功能 + Bug 修復）
│   └── BUG/                  # Bug 分析報告
└── test/                     # 代碼測試文件
```

---

## 新功能開發各階段

| 階段 | 負責方 | 產出 | 門禁 |
|------|--------|------|------|
| 需求 | 用戶 | 需求描述 | — |
| PRD 生成 | AI Agent | `docs/PRD/vX.Y_NAME.md` | — |
| ⚠️ 用戶審批 | 用戶 | ✅ / ❌ | **門禁 1** |
| 技術方案 | AI Agent | `docs/TECH/vX.Y_NAME.md` | — |
| ⚠️ 用戶審批 | 用戶 | ✅ / ❌ | **門禁 2** |
| 測試用例 | AI Agent | `docs/TEST/TEST_CASES_vX.Y.md` + `test/` | — |
| ⚠️ 用戶審批 | 用戶 | ✅ / ❌ | **門禁 3** |
| 實現 | AI Agent | 功能代碼 | — |
| AI 自測 | AI Agent | analyze + test + build + console | — |
| 交付 | AI Agent → 用戶 | SSOT 驗證 + 自測報告 | **交付驗證** |

---

## Bug 修復各階段

| 階段 | 負責方 | 產出 | 門禁 |
|------|--------|------|------|
| 問題 | 用戶 | Bug 描述 + 複現步驟 | — |
| Bug 分析報告 | AI Agent | `docs/BUG/vX.Y_BUG_NAME.md` | — |
| ⚠️ 用戶審批 | 用戶 | ✅ / ❌ | **門禁 1** |
| Bug 修復技術方案 | AI Agent | `docs/TECH/vX.Y_BUGFIX_NAME.md` | — |
| ⚠️ 用戶審批 | 用戶 | ✅ / ❌ | **門禁 2** |
| 測試用例 | AI Agent | `docs/TEST/TEST_CASES_BUGFIX_vX.Y.md` + `test/` | — |
| ⚠️ 用戶審批 | 用戶 | ✅ / ❌ | **門禁 3** |
| 實現 | AI Agent | 修復代碼 | — |
| AI 自測 | AI Agent | analyze + test + build + console + 複現驗證 + 同類掃描 | — |
| 交付 | AI Agent → 用戶 | SSOT 驗證 + 自測報告 | **交付驗證** |

> **Bug 修復額外要求**：AI 自測階段必須額外執行 Bug 複現驗證和同類問題掃描（藍軍自攻擊）。

---

## Bug 嚴重等級

| 等級 | 含義 | 響應時限 |
|------|------|---------|
| P0 | 核心崩潰 / 數據丟失 / 安全漏洞 | 立即修復 |
| P1 | 核心功能無法使用 | 4 小時內 |
| P2 | 非核心功能異常 | 24 小時內 |
| P3 | UI/UX 小問題 | 72 小時內 |

---

## 審批狀態標記

| 狀態 | 標記 | 含義 |
|------|------|------|
| 起草中 | 🔄 | 文檔正在編寫 |
| 待審批 | ⚠️ | 完成，等待用戶審批 |
| 已通過 | ✅ | 用戶審批通過，可進入下一階段 |
| 已拒絕 | ❌ | 需修改後重新提交審批 |

---

## AI 自測強制檢查項

每次實現完成後，AI Agent 必須執行並輸出結果：

```bash
# 1. 靜態代碼分析
flutter analyze

# 2. 單元 / Widget 測試
flutter test

# 3. 構建驗證
flutter build ios --simulator --no-codesign

# 4. Console 錯誤檢查
xcrun simctl spawn "iPhone 16 Pro" log show --predicate 'subsystem == "Flutter"' --last 1m 2>&1 | grep -i error
```

**Bug 修復額外檢查：**
```bash
# 5. Bug 複現驗證（按原始步驟操作，確認 Bug 不再出現）

# 6. 同類問題掃描（藍軍自攻擊）
grep -r "{問題關鍵詞}" lib/
```

---

## 三條紅線

| 紅線 | 內容 |
|------|------|
| 閉環意識 | 聲稱完成前必須跑驗證命令並貼輸出 |
| 事實驅動 | 未經驗證的歸因等於甩鍋，所有結論必須有可複現證據 |
| 窮盡一切 | 未走完完整流程禁止放棄 |

---

## 文檔命名規則

| 類型 | 路徑 | 命名規則 |
|------|------|---------|
| PRD | `docs/PRD/` | `v{major}.{minor}_{NAME}.md` |
| 技術方案 | `docs/TECH/` | `v{major}.{minor}_{NAME}.md` |
| Bug 分析報告 | `docs/BUG/` | `v{major}.{minor}_BUG_{NAME}.md` |
| Bug 修復方案 | `docs/TECH/` | `v{major}.{minor}_BUGFIX_{NAME}.md` |
| 測試用例 | `docs/TEST/` | `TEST_CASES_v{major}.{minor}_{NAME}.md` |
| Bug 測試用例 | `docs/TEST/` | `TEST_CASES_BUGFIX_v{major}.{minor}_{NAME}.md` |
| 代碼測試 | `test/` | `{feature}_test.dart` |

---

## Owner

AI Agent
