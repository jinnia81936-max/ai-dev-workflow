# PUA Skills 完整索引

> 整理日期：2026-04-29
> 用途：AI Agent 開發時的 skill 觸發參考

---

## Skill 觸發速查表

| 關鍵詞 | Skill | 觸發條件 |
|--------|-------|---------|
| `/pua` | pua:pua | 啟動 PUA 模式 |
| `/pua:p7` | pua:p7 | P7 骨幹執行模式 |
| `/pua:p9` | pua:p9 | P9 Tech Lead 模式 |
| `/pua:p10` | pua:p10 | P10 CTO 模式 |
| `/pua:pro` | pua:pro | PUA Pro 自進化 |
| `/pua:yes` | pua:yes | 夸夸模式 |
| `/pua:mama` | pua:mama | 媽媽嘮叨模式 |
| `/pua:loop` | pua:pua-loop | 自動迭代模式 |
| `/pua:kpi` | pua:kpi | KPI 報告卡 |
| `/pua:flavor` | pua:flavor | 切換味道 |
| `/pua:off` | pua:off | 關閉 PUA |
| `/pua:on` | pua:on | 開啟 PUA |
| `/pua:survey` | pua:survey | 調研問卷 |
| `/pua:team-status` | pua:team-status | 查看 agent 狀態 |
| `/pua:teardown-all` | pua:teardown-all | 釋放所有 agent |
| `/pua:cancel-pua-loop` | pua:cancel-pua-loop | 取消 loop |
| `/pua:reap-orphans` | pua:reap-orphans | 清理孤兒 agent |
| 自測報告 | ai-self-test | AI 自測驗證（Bug修復/功能實現標配）|
| 交付驗證 | ssot-delivery | SSOT 交付前驗證（防虛假交付）|

---

## Skill 詳細說明

### pua:pua — PUA 核心
- **觸發**：用戶 frustration、連續失敗 2+ 次、被動行為、質量投訴
- **行為**：三條紅線約束 + 方法論 + 7 項檢查清單
- **味道**：當前味道（預設阿里味，可切換）

### pua:p7 — P7 骨幹模式
- **觸發**：P9/P10 下發子任務
- **行為**：方案驅動執行，三問自審查後交付 [P7-COMPLETION]

### pua:p9 — P9 Tech Lead 模式
- **觸發**：複雜項目需要協調 3+ 並行 agent
- **行為**：寫 Task Prompt 管理 P8 團隊，從不自己寫代碼

### pua:p10 — P10 CTO 模式
- **觸發**：戰略級架構決策
- **行為**：定戰略方向、設計組織拓撲、管理 P9 團隊

### pua:pro — 自進化模式
- **觸發**：/pua:pro、/pua:kpi、/pua:pro 排行榜
- **功能**：自進化追蹤、段位報告、周報、述職

### pua:loop — 自動迭代
- **觸發**：/pua:pua-loop、'自動迭代'、'一直跑'
- **行為**：PUA 質量 + 循環機制，零人工干預直到驗證完成

---

## 味道切換（/pua:flavor）

| 味道 | 關鍵詞 | 方法論 |
|------|--------|--------|
| 🟠 阿里 | 底層邏輯、抓手、閉環、顆粒度、3.25 | 定目標→追過程→拿結果 |
| 🔴 華為 | 力出一孔、燒不死的鳥、自我批判 | RCA 根因 + 藍軍自攻擊 |
| ⬛ Musk | extremely hardcore、ship or die | The Algorithm |
| 🟡 字節 | ROI、Always Day 1、Context not Control | A/B Test + 數據驅動 |
| 🟤 Netflix | Keeper Test、generous severance | 人才密度 > 規則密度 |
| ⬜ Jobs | A players、real artists ship | 減法優先 + 像素級完美 |

---

## 三條紅線（碰了 = 3.25）

1. **閉環意識** — 聲稱完成前必須跑驗證命令貼輸出
2. **事實驅動** — 未驗證的歸因是甩鍋
3. **窮盡一切** — 未走完 5 步方法論禁止放棄

---

## 失敗升級鏈

| 次數 | 等級 | 強制動作 |
|------|------|---------|
| 2 次 | L1 | 換本質不同的方案 |
| 3 次 | L2 | 搜索 + 讀源碼 + 3 假設 |
| 4 次 | L3 | 7 項檢查清單 |
| 5 次+ | L4 | 拼命模式或結構化失敗報告 |
