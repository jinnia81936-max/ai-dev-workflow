# Flutter 開發 Workflow Skills

> 整理日期：2026-04-29
> 更新日期：2026-04-30
> 用途：Flutter App 開發時的 skill 觸發參考（通用版）

---

## 一、觸發速查表

| 關鍵詞 | Skill | 觸發條件 |
|--------|-------|---------|
| Code Connect | figma:figma-code-connect | Figma 組件映射到代碼 |
| 設計系統規則 | figma:figma-create-design-system-rules | 自定義設計規範 |
| 生成圖表 | figma:figma-generate-diagram | 流程圖/架構圖 |
| 生成設計 | figma:figma-generate-design | 從代碼推到 Figma |
| 設計庫 | figma:figma-generate-library | 設計系統變量/組件庫 |
| 實現設計 | figma:figma-implement-design | Figma 設計落地到代碼 |
| 使用 Figma | figma:figma-use | **使用 use_figma 前必須先加載** |

---

## 二、Figma Skill 調用紀律

> ⚠️ **強制規則**：使用 `use_figma` 工具前，**必須**先加載 `figma:figma-use` skill。

```
觸發場景                              → 必須加載的 Skill
─────────────────────────────────────────────────────────
生成流程圖/架構圖                      → figma:figma-generate-diagram
從代碼創建 Figma 設計                  → figma:figma-generate-design
Figma 設計落地到代碼                    → figma:figma-implement-design
建立設計系統（變量/組件庫）             → figma:figma-generate-library
映射 Figma 組件到代碼                   → figma:figma-code-connect
任何 use_figma 寫操作                   → figma:figma-use（MANDATORY）
FigJam 上下文操作                       → figma:figma-use-figjam
```

---

## 三、常用 Flutter 開發命令

```bash
# 分析
flutter analyze              # 代碼分析，0 issues 才算過

# 測試
flutter test                 # 全量測試
flutter test test/xxx.dart   # 單個測試文件

# iOS 構建
flutter build ios --simulator --no-codesign

# Android 構建
flutter build apk --debug

# Simulator 操作
xcrun simctl boot "iPhone 16 Pro"
xcrun simctl install booted build/ios/iphonesimulator/Runner.app
xcrun simctl launch booted com.example.app
xcrun simctl io booted screenshot /tmp/shot.png
xcrun simctl terminate booted com.example.app

# Device Console 錯誤檢查
xcrun simctl spawn "iPhone 16 Pro" log show --predicate 'subsystem == "Flutter"' --last 1m 2>&1 | grep -i error
```

---

## 四、標準 Flutter 項目結構

```
{project_name}/
├── lib/
│   ├── main.dart              # 入口
│   ├── app/                   # 應用層（app.dart, bindings/）
│   ├── ui/                    # UI 層（pages/, widgets/）
│   ├── data/                  # 數據層（models/, providers/, repositories/）
│   ├── routes/                # 路由（app_routes.dart, app_pages.dart）
│   ├── theme/                 # 主題（app_theme.dart）
│   └── translations/          # 翻譯（app_translations.dart）
├── test/                      # 測試文件
└── docs/                      # 文檔（PRD/, TEST/, AI/）
```

---

## 五、GetX 紀律

| 模式 | 說明 |
|------|------|
| `Get.put()` | 全域單例，permanent:true |
| `Get.lazyPut()` | 延遲初始化 |
| `Get.find()` | 獲取已註冊的實例 |
| `Get.isRegistered<T>()` | 檢查是否已註冊 |

> **注意**：某些 Controller 由頁面自己在 initState 中創建，**不通過** GetX 容器，以避免時序問題。具體由項目架構決定。

---

## 六、自動化測試要求

| 測試類型 | 覆蓋場景 |
|---------|---------|
| 單元測試 | 數據模型、格式化邏輯、Controller 邏輯 |
| Widget 測試 | UI 組件交互、事件處理 |
| 集成測試 | 完整頁面流程 |
| 回歸測試 | 不破壞現有功能 |

---

## 七、Owner

AI Agent (Claude Code)
