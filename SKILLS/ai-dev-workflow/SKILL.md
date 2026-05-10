# AI Development Workflow Skill (Strict Enforcement Version)

## Summary

**MANDATORY workflow for AI-driven Flutter development. Violating any constraint = incomplete work. No exceptions.**

**Use when**: Developing Flutter features, fixing bugs, debugging, or about to say "done" to user.

---

## When to Use

| Situation | Action |
|-----------|--------|
| Starting new Flutter feature | Load this skill, follow Development Loop |
| Fixing a bug | Load this skill, follow Bug Fix Loop |
| Debugging error | Load this skill, follow Debug Flow |
| About to say "done" / "完成" / "好了" | STOP → Execute full SSOT Verification first |

---

## ⚠️ CRITICAL ENFORCEMENT RULES

### Rule 1: The "Done" Rule

**You MUST see the evidence before claiming completion.**

```
❌ FORBIDDEN: "Done" / "完成了" / "好了" without showing tool outputs
✅ MANDATORY: Say "done" ONLY after showing:
   1. flutter analyze output showing "0 issues"
   2. flutter test output showing "X/Y passed"
   3. ls output showing file exists
   4. wc -c output showing file size > 100 bytes
   5. Read output showing content exists
```

**If you only show text descriptions without actual command outputs, you have NOT delivered.**

### Rule 2: The Sequential Verification Rule

**Verification steps CANNOT be reordered or skipped.**

```
Write tool → ls → wc -c → Read → Reply to user

This sequence is MANDATORY and UNBREAKABLE.
- ls MUST be executed AFTER Write tool
- wc -c MUST be executed AFTER ls
- Read MUST be executed AFTER wc -c
- Reply to user MUST come AFTER Read
```

### Rule 3: The "Good News" Rule

**When user sounds disappointed, you MUST verify more rigorously, not less.**

```
User: "好了沒？" / "Done?" / "好了吗？"
Your response: MUST show ls/wc output FIRST, THEN confirm status
NEVER skip verification when user is waiting/pushing
```

### Rule 4: The "Screenshot ≠ Evidence" Rule

```
❌ Screenshot of UI = NOT sufficient evidence
❌ "Build succeeded" in text = NOT sufficient evidence
✅ Actual command output (flutter analyze, flutter test, ls, wc) = EVIDENCE
```

---

## Core Principles (MUST FOLLOW)

| Principle | What It Means | Violation = |
|-----------|---------------|-------------|
| **Owner Mindset** | This is YOUR bug, YOUR feature | Not your problem = 3.25 |
| **Test-First** | Write test BEFORE code | Skipping test = incomplete |
| **SSOT Verification** | Tools > your claims | No output = fake delivery |
| **Root Cause Analysis** | Find why, not what to change | Symptom fix = recurrence |
| **Same-Type Scan** | Fix A, check B/C | Ignoring同类 = future bugs |

---

## Workflow 1: Development Loop (STRICT)

```
Receive → Goal → PRD → Tests → Implement → Self-Test → SSOT Verify → Deliver
    ↑                                                                              ↓
    ←←←←←←←←←←←←←←←←←←←  Retrospective ←←←←←←←←←←←←←←←←←←←←←←←←
```

### Step 1: Receive Requirement
- Clarify ambiguous points BEFORE acting
- Ask Owner 4 Questions:
  1. What is the ROOT CAUSE of this problem?
  2. Who else will be affected?
  3. How to prevent next time?
  4. Where is the data?

### Step 2: Define Goal
- Break into smallest executable tasks
- One task = one goal
- Identify dependencies

### Step 3: Align PRD
- New feature → New PRD version (vX.Y)
- Bug fix → Update existing PRD defect record
- Document update → Sync status/execution record

### Step 4: Generate Test Cases (MANDATORY - Test-First)

**This step CANNOT be skipped. Ever.**

Define acceptance criteria BEFORE writing any code:

```dart
group('{Feature} Tests', () {
  test('{test description}', () {
    // Given: Preconditions
    final input = {...};

    // When: Execute operation
    final result = functionUnderTest(input);

    // Then: Verify result
    expect(result, expectedOutput);
  });
});
```

Test location: `test/{feature}_test.dart`

### Step 5: Implement

**Rules MANDATORY during implementation:**
- Analyze before touching code
- Code + tests delivered TOGETHER
- No regression (existing tests still pass)
- Every code change has a purpose

### Step 6: AI Self-Test Verification (MANDATORY - 5 steps)

**All 5 steps MUST be executed. Order CANNOT change. Output MUST be shown.**

```bash
# STEP 1: Code Analysis
flutter analyze
# You MUST see: "0 issues"

# STEP 2: Run Tests
flutter test
# You MUST see: "All X tests passed" (not just "tests passed", must see count)

# STEP 3: iOS Build
flutter build ios --simulator --no-codesign
# You MUST see: "Built build/ios/iphonesimulator/Runner.app" or similar success message

# STEP 4: Console Error Check (MANDATORY - screenshot CANNOT detect this)
xcrun simctl spawn "iPhone 16 Pro" log show --predicate 'subsystem == "Flutter"' --last 1m 2>&1 | grep -i error
# You MUST see: 0 errors OR no output (empty = pass)
# If you see ANY "error" in output = FAIL, must fix

# STEP 5: Regression Test
flutter test
# All existing tests STILL pass
```

**Verification Complete ONLY when ALL 5 steps show passing output.**

### Step 7: SSOT Delivery Verification (MANDATORY - Mechanical)

**This is your final gate before claiming completion.**

#### The Mechanical Sequence (UNBREAKABLE):

```bash
# 1. AFTER Write tool executed:
ls -la {file_path}
# Output MUST show file exists

# 2. IMMEDIATELY after ls:
wc -c {file_path}
# Output MUST show > 100 bytes

# 3. IMMEDIATELY after wc:
head -3 {file_path}
# Output MUST show actual content

# 4. ONLY NOW can you reply to user
```

#### Delivery Checklist (ALL must be checked):

| Check | Evidence Required | If Missing |
|-------|-------------------|------------|
| Code written | ls shows file in correct directory | Not delivered |
| Code non-empty | wc -c shows > 100 bytes | Not delivered |
| Code valid | head -3 shows meaningful content | Not delivered |
| Analyze pass | flutter analyze shows 0 issues | Not delivered |
| Tests pass | flutter test shows X/Y passed | Not delivered |
| Build pass | Build success message visible | Not delivered |
| Console clean | grep error shows 0 errors | NOT delivered |

#### Correct Delivery Format:

```
✅ 已交付

驗證輸出：
- ls: drwxr-xr-x  6 oscar  staff   192 May 10 16:00 docs/PRD/v1.0_feature.md
- wc -c: 1234 docs/PRD/v1.0_feature.md  
- head: # Feature PRD v1.0
- flutter analyze: 0 issues
- flutter test: 20/20 passed
- iOS build: Built build/ios/iphonesimulator/Runner.app
- Console check: 0 errors
```

#### FORBIDDEN Delivery Format:

```
❌ "已交付" (without showing verification outputs)
❌ "完成了" (without showing command outputs)
❌ "好了" (without showing evidence)
❌ "文件已生成" (no ls shown)
❌ "測試通過" (no test output shown)
```

### Step 8: Retrospective

| Step | Question |
|------|----------|
| Review | What did user want? What were acceptance criteria? |
| Evaluate | What was actually delivered? Gap analysis? |
| Analyze | What was root cause of detours? |
| Document | What SOP can be reused? |

---

## Workflow 2: Bug Fix Loop (STRICT)

```
Receive Bug → Confirm → RCA → Design Fix → Code Fix → SSOT Verify → Self-Test → Same-Type Scan → Report
```

### Step 1: Receive Bug
- Bug severity: P0/P1/P2/P3
- Complete reproduction steps
- Scope of impact

### Step 2: Root Cause Analysis (5-Why)

**DO NOT fix symptoms. Fix root cause.**

```
Example:
Bug: Search doesn't work

Why 1: Why search fails?
→ onChanged receives empty string

Why 2: Why onChanged empty?
→ controller not bound to TextField

Why 3: Why controller not bound?
→ Anonymous controller used, not member variable

Why 4: Why anonymous?
→ Developer simplified variable declaration

Why 5: Why simplified without understanding?
→ Unaware of TextField controller lifecycle requirement

Root Cause: TextField requires controller with proper StatefulWidget lifecycle
```

### Step 3: Fix Design
- Will fix introduce new problems?
- Scope of change controllable?
- Consistent with existing architecture?
- Better solution exists?

### Step 4: Code Fix

**MANDATORY: After Write tool, immediately run:**

```bash
ls -la {file_path}
wc -c {file_path}
head -3 {file_path}
```

### Step 5: SSOT Verification

**MUST see ALL outputs before claiming fix complete.**

### Step 6: Self-Test (5 commands, ALL mandatory)

```bash
# 1. Analyze
flutter analyze
# Must see: 0 issues

# 2. Test
flutter test
# Must see: All X tests passed

# 3. Build
flutter build ios --simulator --no-codesign
# Must see: Success message

# 4. Console Error Check (MANDATORY)
xcrun simctl spawn "iPhone 16 Pro" log show --predicate 'subsystem == "Flutter"' --last 1m 2>&1 | grep -i error
# Must see: 0 errors

# 5. Regression
flutter test
# All tests still pass
```

### Step 7: Same-Type Scan (Blue Army Attack)

After fixing bug A, check:

```bash
# Search for similar patterns
grep -r "same_problematic_pattern" lib/
```

Checklist:
- [ ] Same module - similar issues?
- [ ] Adjacent modules - affected?
- [ ] Edge cases - handled?
- [ ] Abnormal input - crashes?

### Step 8: Delivery Report

**Report template at end of this document.**

---

## Workflow 3: Debug Flow (STRICT)

```
发现问题 → 闻味道 → 5-Why → 7项检查 → 修复 → 验证 → 同类扫描 → 预防
```

### Debug Routing by Failure Count:

| Failures | Level | Method |
|----------|-------|--------|
| 0-1 | L0 | 闻味道 → 5-Why |
| 2 | L1 | Musk → Pinduoduo |
| 3 | L2 | Baidu → Amazon |
| 4 | L3 | **7-Item Checklist MANDATORY** |
| 5+ | L4 | Desperation + structured failure report |

### 7-Item Checklist (L3+ MUST complete):

- [ ] Read failure signal word-by-word?
- [ ] Searched core issue with tools?
- [ ] Read original context?
- [ ] Verified all assumptions with tools?
- [ ] Tried completely opposite assumption?
- [ ] Can reproduce in minimal scope?
- [ ] Changed tools/methods/angles/tech stack?

---

## Anti-Fake Delivery Patterns (DETECT AND STOP)

**When you detect these patterns, IMMEDIATELY execute verification:**

### Trigger Patterns:

```
❌ User says "好了沒？" / "Done?" / "好了吗？" → You MUST show verification output FIRST
❌ "已交付" without ls output → STOP, show ls
❌ "已完成" without wc output → STOP, show wc
❌ "測試通過" without test output → STOP, show test
❌ "Build成功" without build output → STOP, show build
❌ "沒有error" without console check → STOP, run console check
```

### Self-Correction Triggers:

When you catch yourself thinking:
- "It's probably fine" → VERIFY
- "The user is waiting, I'll say done" → VERIFY FIRST
- "This small change doesn't need full testing" → VERIFY
- "Screenshot looks good" → RUN CONSOLE CHECK

---

## Red Lines (VIOLATION = INCOMPLETE WORK)

| Red Line | What Violates It |
|----------|------------------|
| Red Line 1 | Saying "done" without showing any tool output |
| Red Line 2 | Claiming file exists without showing ls output |
| Red Line 3 | Claiming tests pass without showing test output |
| Red Line 4 | Claiming build success without showing build output |
| Red Line 5 | Claiming "no errors" without running console check |
| Red Line 6 | Skipping verification when user sounds impatient |
| Red Line 7 | Using screenshot as sole evidence of functionality |

---

## Document Directory Standards (MANDATORY)

```
docs/
├── PRD/          ← All PRD documents (feature specs)
├── TECH/         ← All technical design documents
├── TEST/         ← All test case documents
└── AI/           ← AI knowledge base (skills/flows/templates)
```

| Type | Location | Naming |
|------|----------|--------|
| PRD | `docs/PRD/` | `v{major}.{minor}_{NAME}.md` |
| Technical | `docs/TECH/` | `v{major}.{minor}_{NAME}.md` |
| Test Cases | `docs/TEST/` | `TEST_CASES_v{major}.{minor}_{NAME}.md` |
| Code Tests | `test/` | `{feature}_test.dart` |

**Rule**: Document in wrong location = document does not exist.

---

## Pre-Delivery Self-Check (MUST COMPLETE BEFORE SAYING "DONE")

Read each item. If answer is NO, you have NOT delivered:

```
□ Did Write tool execute for this delivery?
□ Did ls output show file exists in CORRECT directory?
□ Did wc -c output show file > 100 bytes?
□ Did head output show meaningful content?
□ Did flutter analyze output show 0 issues?
□ Did flutter test output show All X tests passed?
□ Did flutter build output show success?
□ Did console check (grep error) show 0 errors?
□ Did I show the actual outputs, not just describe them?
```

**All boxes checked = You may say "done"**
**Any box unchecked = You have NOT delivered**

---

## Owner Mindset 4 Questions (ASK BEFORE DELIVERY)

1. **What is the ROOT CAUSE?** (not "how to fix", but "why did it happen")
2. **Who else will be affected?** (changing A, will B/C break?)
3. **How to PREVENT next time?** (can we add check/lint/test?)
4. **Where is the DATA?** (judgment supported by evidence?)

---

## Report Templates

### Feature Delivery Self-Test Report:

```markdown
## AI Self-Test Report: {Feature Name}

**Date**: {YYYY-MM-DD HH:MM}

### Verification Results

| Check | Output | Status |
|-------|--------|--------|
| ls | {actual output} | ✅/❌ |
| wc -c | {actual output} bytes | ✅/❌ |
| head | {first 3 lines} | ✅/❌ |
| flutter analyze | {X issues} | ✅/❌ |
| flutter test | {X/Y passed} | ✅/❌ |
| iOS build | {success message} | ✅/❌ |
| Console check | {X errors} | ✅/❌ |

### Conclusion

{All pass → "✅ HIGH QUALITY DELIVERY" / Any fail → "❌ INCOMPLETE"}
```

### Bug Fix Self-Test Report:

```markdown
## Bug Fix Self-Test Report: {Bug ID}

**Date**: {YYYY-MM-DD HH:MM}
**Bug Level**: {P0/P1/P2/P3}

### Root Cause

```
Why 1: {reason}
Why 2: {reason}
Why 3: {reason}
Why 4: {reason}
Why 5: {reason}
Root Cause: {final root cause}
```

### Verification Results

| Check | Output | Status |
|-------|--------|--------|
| flutter analyze | {X issues} | ✅/❌ |
| flutter test | {X/Y passed} | ✅/❌ |
| iOS build | {success message} | ✅/❌ |
| Console check | {X errors} | ✅/❌ |
| Regression | {status} | ✅/❌ |
| Same-type scan | {result} | ✅/❌ |

### Conclusion

{All pass → "✅ BUG FIXED" / Any fail → "❌ INCOMPLETE"}
```

---

## What Counts as Evidence

| Type | Evidence | NOT Evidence |
|------|----------|--------------|
| File exists | `ls -la` output | "file exists" in text |
| File has content | `wc -c` + `head` output | "file has content" in text |
| Analyze passes | "0 issues" in output | "analyze passed" in text |
| Tests pass | "X/Y passed" in output | "tests passed" in text |
| Build succeeds | Success message in output | "build succeeded" in text |
| No runtime errors | 0 errors in console check | "no errors" in text |
| Screenshot | Supplementary only | Primary evidence |

---

**Owner**: AI Agent (Claude Code)

**This skill enforces strict delivery standards. Violating any constraint = incomplete work.**