# BMad Hacker Daily Digest 測試策略

## 整體理念與目標

BMad Hacker Daily Digest 的 MVP 測試策略聚焦於實用且務實地驗證核心管線功能與各個元件的邏輯。由於此為一個具順序流程的本地 CLI 工具，因此測試重點包含：

1.  **功能正確性：** 確保管線中每一個階段（抓取、擷取、摘要、寄信）都能依據需求正確運作。
2.  **整合驗證：** 確保資料可透過本地檔案系統在階段之間正確傳遞。
3.  **韌性（關鍵區段）：** 特別針對預期錯誤（如文章擷取失敗）是否能被優雅處理進行測試。
4.  **活用 Boilerplate：** 使用 `bmad-boilerplate` 中提供的 Jest 測試框架進行自動化單元與整合測試。
5.  **階段驗證為主：** 透過 **階段測試工具（Stage Testing Utilities）** 作為驗證每個階段與實際外部互動是否成功的主要方式。

此測試策略的主要目標，是在不追求全面覆蓋率的前提下，確保 MVP 的端對端執行結果正確且可信。高覆蓋率並非優先，重要的是關鍵邏輯與整合節點的測試。

## 測試層級

### 單元測試

- **範疇：** 單獨測試每個函式、方法或模組邏輯。聚焦於工具函式（如 `src/utils/`）、Client 模組（如 `src/clients/`，需 mock HTTP 請求）、擷取邏輯（如 `src/scraper/`）、Email 模板（如 `src/email/templates.ts`）、或核心管線邏輯（如 `src/core/pipeline.ts`）。
- **工具：** 使用 `bmad-boilerplate` 提供的 Jest。執行 `npm run test`。
- **Mocking／Stubbing：** 利用 Jest 的 mock 工具（如 `jest.fn()`、`jest.spyOn()`、`__mocks__` 目錄）隔離測試單元，避免與外部依賴（如原生 Workspace API、`fs`、第三方套件如 `nodemailer`、`ollamaClient`）直接互動。
- **位置：** `test/unit/`，結構需與 `src/` 對應。
- **預期：** 覆蓋重要邏輯分支、計算與輔助函式。測試應快速、穩定。工具函式與複雜模組邏輯需有良好覆蓋。

### 整合測試

- **範疇：** 驗證模組間互動正確。例如：
  - 測試 `core/pipeline.ts` 內部流程是否能正確呼叫 mock 階段實作。
  - 測試 `algoliaHNClient` 對 mock HTTP 回應是否能正確解析。
  - 測試 `email/contentAssembler.ts`，對 mock 資料是否能正確產生 DigestData。
- **工具：** 使用 Jest，搭配必要的測試前後置邏輯建立模擬檔案結構（如 mock-fs）。
- **位置：** `test/integration/`
- **預期：** 驗證模組間協作與介面契約，測試速度較慢但涵蓋模組邊界。

### 端對端（E2E）／接受測試（使用階段工具）

- **範疇：** 這是驗證各個主要階段與實際外部互動是否成功的 **主要測試方式**，並包含完整管線執行。
- **流程：**
  1.  **階段測試工具：** 透過 `src/stages/` 內的腳本進行：
      - `npm run stage:fetch`：驗證是否能成功從 Algolia HN API 抓取資料並儲存 `_data.json`。
      - `npm run stage:scrape`：驗證是否能根據 `_data.json` 擷取實際文章內容並存為 `_article.txt`。
      - `npm run stage:summarize`：驗證是否能呼叫本地 Ollama API，生成摘要存為 `_summary.json`（需有運行中的本地 Ollama）。
      - `npm run stage:email [--dry-run]`：驗證是否能讀取本地資料，組裝 HTML 並寄出 email 或輸出 HTML 預覽（需正確設定 `.env`）。
  2.  **完整管線執行：** 透過 `npm run dev` 或 `npm start` 執行整體流程。
  3.  **人工驗證：** 查看 console log 是否有錯誤，檢查 `output/YYYY-MM-DD/` 目錄下是否有正確格式與內容的輸出檔案，檢查 email（若 live 測試）是否格式與摘要正確。
- **工具：** `npm` 指令、console、檔案與 email 驗證。
- **環境：** 本地開發機，需能連線外網、有 `.env` 與本地 Ollama 執行中。
- **位置：** 腳本位於 `src/stages/`，驗證步驟以人工為主。
- **預期：** 驗證真實場景中每個階段與整體流程是否可成功完成，符合 MVP 的成功標準。

### 人工／探索性測試

- **範疇：** 聚焦於 email digest 的主觀品質，如 HTML 排版可讀性、摘要邏輯連貫與資訊密度。
- **流程：** 閱讀 `stage:email` 輸出結果（`_digest_preview.html` 或收到的 email）。

## 專門類型測試

- MVP 階段不涵蓋效能、安全性、無障礙等測試。

## 測試資料管理

- **單元／整合測試：** 使用硬編寫測資、Jest mock、或模擬檔案系統。
- **階段／端對端測試：** 使用即時抓取資料，或使用前一階段產生之中繼檔案。`stage:email` 可用 `--dry-run` 減少寄信次數。

## CI/CD 整合

- MVP 不含 CI。若日後導入，CI 可執行 `npm run lint`、`npm run test`。但階段測試需考量外部依賴與速率限制，較不建議於 CI 執行。

## 變更紀錄

| 變更內容      | 日期        | 版本    | 說明                      | 作者         |
| ------------- | ----------- | ------- | ------------------------- | ------------ |
| 初始草稿      | 2025-05-04  | 0.1     | 依據 PRD 與架構草擬初稿   | 3-Architect  |
