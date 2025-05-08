# BMad Hacker Daily Digest 程式碼標準與設計模式

本文件說明在 BMad Hacker Daily Digest 專案開發中應遵循的程式碼標準、設計模式與最佳實務。遵守這些規範對於程式的可維護性、可讀性與團隊協作至關重要。

## 架構／設計模式

- **線性管線（Sequential Pipeline）：** 主應用程式依序執行一系列步驟（抓取、擷取、摘要、寄送 email），由 `src/core/pipeline.ts` 協調管理。
- **模組化設計：** 將應用程式根據職責分割為獨立模組（如 `clients/`, `scraper/`, `email/`, `utils/`），以促進職責分離、可測試性與可維護性。詳見 `docs/project-structure.md`。
- **Client 抽象：** 所有對外服務互動（如 Algolia、Ollama）皆封裝於 `src/clients/` 中，方便替換與測試。
- **檔案系統中繼資料：** 管線階段間透過本地檔案交接資料，而非資料庫，簡化實作並利於除錯。

## 程式碼標準

- **主要語言：** TypeScript（v5.x，依 boilerplate 設定）
- **執行環境：** Node.js（v22.x，依 PRD 要求）
- **程式風格與靜態檢查：** 使用 ESLint 與 Prettier，由 `bmad-boilerplate` 提供設定。
  - **強制規定：** 定期執行 `npm run lint` 與 `npm run format`，提交前須無任何 lint 錯誤。
- **命名規則：**
  - 變數與函式：`camelCase`
  - 類別、型別、介面：`PascalCase`
  - 常數：`UPPER_SNAKE_CASE`
  - 檔案名稱：以 `kebab-case.ts`（例如 `article-scraper.ts`）或 `camelCase.ts`（如 `ollamaClient.ts`）為主。模組內部命名應保持一致。推薦使用 `camelCase.ts` 來與類別／模組名對齊，工具或階段腳本則採 `kebab-case.ts`（如 `Workspace-hn-data.ts`）。
  - 測試檔案：`*.test.ts`（例如 `ollamaClient.test.ts`）
- **檔案結構：** 嚴格遵循 `docs/project-structure.md` 中定義的專案架構。
- **非同步處理：** **強制規定：** 所有 async 操作均使用 `async`/`await`，不得使用 `.then()`／`.catch()` 除非必要。
- **型別安全：** 積極使用 TypeScript 靜態型別，適當使用 `src/types/` 中定義的型別。假設 `tsconfig.json` 中啟用了 `strict` 模式。除非有正當理由，不可使用 `any`。
- **註解與文件：**
  - 對外導出的函式、類別與複雜邏輯應使用 JSDoc 註解。
  - 註解應著重說明「為什麼」，非「做什麼」，除非邏輯複雜。
  - 如有變動，請同步更新 README 或相關文件。
- **相依管理：**
  - 使用 `npm` 管理套件。
  - 僅加入必要的 production 套件，並說明用途。
  - 測試、lint、建置工具放入 `devDependencies`。

## 錯誤處理策略

- **基本原則：** 對可能失敗的操作（I/O、網路請求、解析）使用標準 `try...catch`。拋出具描述性的 `Error` 物件。除非明確處理，不可 catch 後無處理或不重拋。
- **日誌：**
  - **強制規定：** 所有輸出皆使用 `src/utils/logger.ts`，禁止直接使用 `console.log`。
  - **格式：** MVP 階段使用純文字格式。未來可考慮改為結構化 JSON 日誌。
  - **等級：** 使用 `logger.info`、`logger.warn`、`logger.error`。
  - **內容：** 記錄相關上下文資訊（如 Story ID、函式名稱、請求 URL）。
- **特定錯誤處理模式：**
  - **外部 API（Algolia、Ollama）：**
    - 包裹於 `try...catch`。
    - 檢查 `response.ok`；如失敗，記錄狀態碼與內容，並視為錯誤。
    - 網路錯誤應記錄於 catch 區塊。
    - MVP 階段無需自動重試。
  - **文章擷取（`articleScraper.ts`）：**
    - 包裹網路請求與擷取邏輯，處理非 2xx 回應、逾時、非 HTML 類型與擷取錯誤。
    - **重要：** 擷取失敗時應記錄錯誤並回傳 `null`，主流程須繼續處理（僅用留言摘要）。
  - **檔案操作（`fs` 模組）：**
    - 包裹所有檔案寫入並記錄錯誤。
  - **寄信（Nodemailer）：**
    - 包裹 `sendMail()`，清楚記錄成功（含 message ID）與失敗。
  - **環境變數載入（`config.ts`）：**
    - 啟動時檢查必要變數，缺少則中止執行並報錯。
  - **LLM 操作（Ollama）：**
    - 使用 `docs/prompts.md` 中定義的標準 prompt。
    - 包裹 `generateSummary` 並記錄 API 或網路錯誤。
    - 若有 `MAX_COMMENT_CHARS_FOR_SUMMARY` 設定，則對留言摘要長度做截斷，並記錄警告。

## 安全性最佳實務

- **輸入驗證：** 雖為本地工具，但仍應驗證如 `story.articleUrl` 這類外部 URL。MVP 可使用簡單格式檢查（是否為 `http://` 或 `https://` 開頭）。
- **機密管理：**
  - **強制規定：** 僅能將敏感資料（如 `EMAIL_USER`, `EMAIL_PASS`）寫入 `.env`。
  - **強制規定：** `.env` 檔案必須列入 `.gitignore`，且不可加入版本控制。
  - 禁止將機密寫入原始碼。
- **依賴安全性：** 定期執行 `npm audit`，可考慮在 GitHub 上啟用 Dependabot。
- **HTTP 客戶端：** 使用原生 `Workspace` API，避免使用額外不必要的 HTTP 套件。
- **抓取用戶代理：** 預設使用 `BMadHackerDigest/0.1`，可透過 `SCRAPER_USER_AGENT` 環境變數覆寫。

## 變更紀錄

| 變更內容      | 日期        | 版本    | 說明                  | 作者         |
| ------------- | ----------- | ------- | --------------------- | ------------ |
| 初始草稿      | 2025-05-04  | 0.1     | 根據架構草稿初步撰寫  | 3-Architect  |
