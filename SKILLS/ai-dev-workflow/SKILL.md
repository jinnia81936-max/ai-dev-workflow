# AI Development Workflow Skill (Approval-Gated Version)

## Summary

**AI-driven Flutter development with mandatory human approval gates. Each phase requires explicit approval before proceeding. No skipping.**

**Workflow**: PRD → Approval → Tech Spec → Approval → Test Cases → Approval → Development → Delivery

---

## When to Use

| Situation | Action |
|-----------|--------|
| Starting new Flutter feature | Start from Phase 1: PRD |
| Continuing after PRD approved | Go to Phase 2: Tech Spec |
| After Tech Spec approved | Go to Phase 3: Test Cases |
| After Test Cases approved | Go to Phase 4: Development |
| Ready for delivery | Execute Phase 5: Acceptance |

---

## ⚠️ CRITICAL ENFORCEMENT RULES

### Rule 1: Approval Gate Rule

**Each phase MUST be approved before next phase begins. No exceptions.**

```
❌ FORBIDDEN: Proceeding to next phase without approval
✅ MANDATORY: Wait for human approval after each phase
```

**Approval Format Required:**
```
✅ [Phase Name] 已完成，等待審批

階段：[名稱]
產出：[文件列表]
下一步：請確認後回复"批准"或"需要修改"
```

### Rule 2: The "Done" Rule

**You MUST see evidence before claiming completion.**

```
❌ FORBIDDEN: "Done" / "完成了" / "好了" without showing tool outputs
✅ MANDATORY: Say "done" ONLY after showing ALL verification outputs
```

### Rule 3: Sequential Verification Rule

**Verification steps CANNOT be reordered or skipped.**

```
Write tool → ls → wc -c → Read → Reply to user
```

### Rule 4: Screenshot ≠ Evidence Rule

```
❌ Screenshot of UI = NOT sufficient evidence
✅ Actual command output (flutter analyze, flutter test, ls, wc) = EVIDENCE
```

---

## Workflow Phases

### Phase 1: Generate PRD

**Owner**: AI Agent → Human Approval

**Steps:**

1. **Analyze Requirement**
   - Clarify ambiguous points
   - Ask Owner 4 Questions:
     1. What is the ROOT CAUSE of this problem?
     2. Who else will be affected?
     3. How to prevent next time?
     4. Where is the data?

2. **Create PRD Document**
   - Location: `docs/PRD/v{major}.{minor}_{NAME}.md`
   - Content:
     - 審批狀態
     - 功能概述（背景問題、重構目標）
     - 用戶場景（核心場景、用戶價值）
     - 詳細設計（架構變更、UI變更、數據模型）
     - **驗收標準**（可量化、可測試）
     - 執行記錄（時間、結果、Owner）

3. **SSOT Verification**
   ```bash
   ls -la docs/PRD/v{major}.{minor}_{NAME}.md
   wc -c docs/PRD/v{major}.{minor}_{NAME}.md
   head -10 docs/PRD/v{major}.{minor}_{NAME}.md
   ```

**Phase Output:**
- PRD file created at correct location
- SSOT verification outputs shown
- Ready for human approval

**Approval Request Format:**
```
✅ PRD 已生成，等待審批

文件：docs/PRD/v{major}.{minor}_{NAME}.md
驗收標準：共 N 項

請確認：
- 功能概述是否準確？
- 驗收標準是否完整可測試？
- 用戶場景是否覆蓋核心流程？

回复"批准"繼續，或"需要修改" + 具體要求
```

---

### Phase 2: Generate Technical Solution

**Owner**: AI Agent → Human Approval

**Prerequisite**: PRD approved

**Steps:**

1. **Analyze PRD**
   - Read approved PRD
   - Understand acceptance criteria
   - Identify technical challenges

2. **Create Technical Solution**
   - Location: `docs/TECH/v{major}.{minor}_{NAME}.md`
   - Content:
     - 現有架構分析
     - 目標架構
     - 具體修改方案（每個文件的變更內容）
     - 數據遷移策略（如有）
     - 測試驗證命令（flutter analyze、build）
     - 執行計劃（步驟清單）

3. **SSOT Verification**
   ```bash
   ls -la docs/TECH/v{major}.{minor}_{NAME}.md
   wc -c docs/TECH/v{major}.{minor}_{NAME}.md
   head -10 docs/TECH/v{major}.{minor}_{NAME}.md
   ```

**Phase Output:**
- Technical solution file created
- SSOT verification outputs shown
- Ready for human approval

**Approval Request Format:**
```
✅ 技術方案已生成，等待審批

文件：docs/TECH/v{major}.{minor}_{NAME}.md
修改文件：共 N 個

請確認：
- 架構變更是否合理？
- 修改方案是否可行？
- 測試驗證命令是否正確？

回复"批准"繼續，或"需要修改" + 具體要求
```

---

### Phase 3: Generate AI Self-Test Test Cases

**Owner**: AI Agent → Human Approval

**Prerequisite**: Technical solution approved

**Steps:**

1. **Analyze Technical Solution**
   - Read approved technical solution
   - Understand file changes
   - Map to acceptance criteria

2. **Create Test Cases**
   - Location: `docs/TEST/TEST_CASES_v{major}.{minor}_{NAME}.md`
   - Location: `test/{feature}_test.dart` (code tests)

   **Test Case Structure:**

   | TC-XXX | 測試項 | 前置條件 | 測試步驟 | 預期結果 | 斷言 |
   |--------|--------|---------|---------|---------|------|
   | TC-001 | 功能描述 | 條件 | 步驟 | 結果 | 斷言 |

   **Test Types Required:**
   - 單元測試（數據模型、Controller邏輯）
   - Widget測試（UI組件交互）
   - 集成測試（完整頁面流程）
   - 回歸測試（不破壞現有功能）

3. **SSOT Verification**
   ```bash
   ls -la docs/TEST/TEST_CASES_v{major}.{minor}_{NAME}.md
   ls -la test/{feature}_test.dart
   wc -c docs/TEST/TEST_CASES_v{major}.{minor}_{NAME}.md
   ```

**Phase Output:**
- Test case document created
- Code test files created
- SSOT verification outputs shown
- Ready for human approval

**Approval Request Format:**
```
✅ 測試用例已生成，等待審批

文檔：docs/TEST/TEST_CASES_v{major}.{minor}_{NAME}.md
代碼：test/{feature}_test.dart
測試項：共 N 項 TC

請確認：
- 驗收標準是否都有對應TC？
- 測試覆蓋是否完整（單元/Widget/集成/回歸）？
- 斷言是否合理可執行？

回复"批准"繼續，或"需要修改" + 具體要求
```

---

### Phase 4: AI Development with Self-Test

**Owner**: AI Agent

**Prerequisite**: Test cases approved

**Steps:**

1. **Implement Code**
   - Write code following technical solution
   - Code + tests delivered TOGETHER
   - No regression (existing tests still pass)

2. **Self-Test Verification (MANDATORY - 5 steps)**

   **All 5 steps MUST be executed. Output MUST be shown.**

   ```bash
   # STEP 1: Code Analysis
   flutter analyze
   # Must see: "0 issues"

   # STEP 2: Run Tests
   flutter test
   # Must see: "All X/Y tests passed"

   # STEP 3: iOS Build
   flutter build ios --simulator --no-codesign
   # Must see: "Built build/ios/iphonesimulator/Runner.app"

   # STEP 4: Console Error Check (MANDATORY)
   xcrun simctl spawn "iPhone 16 Pro" log show --predicate 'subsystem == "Flutter"' --last 1m 2>&1 | grep -i error
   # Must see: 0 errors

   # STEP 5: Regression
   flutter test
   # All existing tests STILL pass
   ```

3. **Test Case Coverage Verification**
   - Each TC must have corresponding test result
   - Report template below

**Phase Output:**
- All code implemented
- All 5 verification steps passed
- Test case coverage 100%

---

### Phase 5: Delivery Acceptance

**Owner**: Human Approval

**Prerequisite**: All self-tests passed

**Steps:**

1. **SSOT Delivery Verification (MANDATORY)**

   ```bash
   # 1. File existence
   ls -la {modified_files}

   # 2. File non-empty
   wc -c {modified_files}

   # 3. Content valid
   head -3 {modified_files}
   ```

2. **Self-Test Report Submission**

   AI submits completed verification report

3. **Human Acceptance**

   Human reviews and approves delivery

**Delivery Format:**

```
✅ 交付驗收

修改文件：
- {file1}
- {file2}

驗證輸出：
- flutter analyze: 0 issues
- flutter test: X/Y passed
- iOS build: Built build/ios/iphonesimulator/Runner.app
- Console check: 0 errors

測試用例覆蓋：
- TC-001: ✅ 已實現
- TC-002: ✅ 已實現
...

請確認驗收
```

---

## Core Principles (MUST FOLLOW)

| Principle | What It Means |
|-----------|---------------|
| **Owner Mindset** | This is YOUR feature, YOUR bug |
| **Approval Gate** | Each phase requires human approval |
| **Test-First** | Write tests BEFORE code |
| **SSOT Verification** | Tools > your claims |
| **100% Coverage** | All TCs must pass |
| **No Shortcuts** | Every step must be executed |

---

## Document Directory Standards (MANDATORY)

```
docs/
├── PRD/          ← All PRD documents (feature specs)
├── TECH/         ← All technical design documents
├── TEST/         ← All test case documents
└── AI/           ← AI knowledge base (skills/flows/templates)

test/             ← Code test files
```

| Type | Location | Naming |
|------|----------|--------|
| PRD | `docs/PRD/` | `v{major}.{minor}_{NAME}.md` |
| Technical | `docs/TECH/` | `v{major}.{minor}_{NAME}.md` |
| Test Cases | `docs/TEST/` | `TEST_CASES_v{major}.{minor}_{NAME}.md` |
| Code Tests | `test/` | `{feature}_test.dart` |

---

## Approval Status Definitions

| Status | Meaning |
|--------|---------|
| 🔄 起草中 | Document being written |
| ⚠️ 待審批 | Completed, waiting for approval |
| ✅ 通過 | Approved, can proceed |
| ❌ 否決 | Rejected, needs revision |

---

## Owner Mindset 4 Questions (ASK BEFORE DELIVERY)

1. **What is the ROOT CAUSE?**
2. **Who else will be affected?**
3. **How to PREVENT next time?**
4. **Where is the DATA?**

---

## Self-Test Report Template

```markdown
## AI Self-Test Report: {Feature Name}

**Date**: {YYYY-MM-DD HH:MM}
**Phase**: 交付驗收

### Verification Results

| Check | Output | Status |
|-------|--------|--------|
| flutter analyze | {X issues} | ✅/❌ |
| flutter test | {X/Y passed} | ✅/❌ |
| iOS build | {success message} | ✅/❌ |
| Console check | {X errors} | ✅/❌ |

### Test Case Coverage

| TC | Test Item | Status |
|----|-----------|--------|
| TC-001 | {description} | ✅/❌ |
| TC-002 | {description} | ✅/❌ |

### Modified Files

| File | LS Output | WC Output |
|------|-----------|-----------|
| {path} | {output} | {bytes} |

### Conclusion

{All pass → "✅ READY FOR ACCEPTANCE" / Any fail → "❌ INCOMPLETE"}
```

---

**Owner**: AI Agent (Claude Code)

**This skill enforces strict approval-gated workflow. No phase may be skipped.**