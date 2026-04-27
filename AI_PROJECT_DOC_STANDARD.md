# AI Project Documentation Standard (Public Release)

> 適用範圍：所有對外發布的 AI 專案 README / 官網技術文件。

## A. 文件整體原則
1. **先價值、後實作**：先說明專案價值與導入效益，再提供技術細節。
2. **可驗證**：所有「可運作」敘述必須對應可重現步驟。
3. **可擴充**：硬體驗證資訊需保留未來新增平台的版面。
4. **圖像化成果**：至少一張執行結果圖（建議兩張以上）。
5. **正式對外語氣**：避免內部溝通用語、口語化提示與不必要註解。
6. **自包含交付**：禁止在 runtime/建置流程依賴「臨時 clone upstream」作為必要步驟；專案需包含可運行所需核心檔案。

---

## B. 固定章節順序（必須）

> 可使用中文章節名或英文章節名（README-style），但內容責任不可缺漏。

### 1) 專案標題與一句話定位
- 專案名稱
- 一句話價值主張（如即時、可部署、可維運）

### 2) 專案資訊
- 專案定位與可解決問題
- 導入效益（時程、風險、維運一致性）

### 3) 模型介紹
- 核心模型（例如 OWL-ViT / CLIP / SAM）
- 推論流程特性（例如 tree prompt、open-vocabulary）
- 加速方式（例如 TensorRT engine）
- 技術來源連結（upstream / paper）

### 4) 相依來源與封裝策略（新增）
- 明確列出「哪些 upstream 檔案已 vendor 進 repo」
- 明確列出「哪些仍為外部依賴（pip/apt）」
- 說明為何可離線重跑，且不需額外 clone

### 5) Edge_AI_SDK / InferenceKit 前置價值（若適用）
- 放置連結：<https://ess-wiki.advantech.com.tw/view/Edge_AI_SDK/InferenceKit>
- 說明先安裝 InferenceKit 的理由與效益

### 6) 已驗證硬體平台（可擴充表格）
- 欄位至少包含：平台、驗證狀態、連結、備註
- 第一列為已驗證平台
- 保留待新增平台列

### 7) 執行結果示意（圖片/GIF）
- 至少 1 張結果圖
- 建議「即時畫面 + 推論輸出」雙圖
- 優先使用 repo 內 `assets/` 路徑，避免外部圖床失效

### 8) 開發者資訊（安裝與執行）
- 前置條件
- 建置步驟
- 第一次啟動
- 第二次啟動（快取重用）
- 離線啟動（若支援）
- 驗證方式
- 常見問題

> 英文對應可用：`Setup` + `Development and Deployment`（Step-by-step）。

### 9) 最低驗收門檻（新增）
- `docker build` 成功（含完整指令與工作路徑）
- `docker compose up -d` 成功
- `docker compose ps -a` 顯示服務 `Up`
- 若為互動式 demo，需提供可視化驗收依據（UI 截圖/影片/可重現步驟）

### 10) 文件規範引用
- 文件結尾必須引用本規範與模板檔

---

## C. 撰寫格式規範
1. 指令必須可直接複製執行。
2. 版本資訊需明確（JetPack / CUDA / TensorRT / image tag）。
3. 避免模糊措辭（如「應該可以」）。
4. 連結需為完整 URL。
5. 圖片路徑需可公開訪問或為 repo 內有效路徑。
6. 避免出現內部備註字樣（如「給採購者/給內部」等）。
7. 所有 command block 需標示執行位置（host / container）與工作路徑。
8. 文件中的成功宣告需對應到可查證輸出（log、commit、ps 狀態）。
9. 若使用英文 README 風格，需至少保留：`Supported Platform`、`Setup`、`Development and Deployment`、`Result` 與驗收命令區塊。

---

## D. 新專案套版（精簡）

```md
# <PROJECT_NAME>

## 專案資訊
### 1) 專案定位
### 2) 模型介紹
### 3) 相依來源與封裝策略
### 4) 為什麼先用 InferenceKit
### 5) 已驗證硬體平台
### 6) 執行結果示意

---

## 開發者資訊
### 0) 前置條件
### 1) 建置
### 2) 第一次啟動
### 3) 第二次啟動
### 4) 離線啟動（可選）
### 5) 驗證
### 6) 常見問題

---

## 最低驗收門檻
- Build 成功
- Compose 服務為 Up
- 視覺化結果可驗證

---

## 文件規範
本文件遵循 `AI_PROJECT_DOC_STANDARD.md`。
```

## E. README-style 英文章節映射（建議）

- 專案標題與一句話定位 → `# <PROJECT_NAME>` + one-line description
- 專案資訊 / 模型介紹 → intro + model feature bullets
- 已驗證硬體平台 → `## Supported Platform`
- 開發者資訊（安裝與執行） → `# Setup` + `# Development and Deployment`
- 執行結果示意 → `## Result`
- 最低驗收門檻 → build/compose/ps command block + expected `Up`
- 文件規範引用 → `## Document Standard`
