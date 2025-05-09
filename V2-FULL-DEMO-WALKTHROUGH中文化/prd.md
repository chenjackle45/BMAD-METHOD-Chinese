# BMad Hacker Daily Digest 產品需求文件（PRD）

## 介紹

BMad Hacker Daily Digest 是一款命令列工具，旨在解決閱讀 Hacker News（HN）冗長留言串所耗費的時間問題。它為使用者提供一種高效掌握 HN 熱門討論集體智慧與關鍵洞見的方式。服務會每日擷取前 10 則 HN 熱門文章，為每篇文章取得可設定數量的留言，嘗試擷取連結文章內容，並分別利用本地 LLM 產生文章摘要（如有擷取成功）與討論摘要，最後將這些摘要彙整於每日單封電子郵件簡報中，並以手動方式觸發寄送。本專案同時作為實作 agent-driven 開發、TypeScript、Node.js、API 整合及本地 LLM 應用的實務學習，並以「bmad-boilerplate」樣板為起點。

## 目標與背景

- **專案目標：**
  - 提供一種快速、可靠、自動化的方式，讓使用者無需閱讀完整留言串即可掌握 HN 重要討論內容。
  - 能夠透過 Algolia HN API 成功擷取前 10 則 HN 文章的基本資料。
  - 能夠透過 Algolia HN API 取得每篇文章可設定數量（預設 50）的留言。
  - 嘗試基礎擷取連結文章內容，並能優雅處理失敗情境。
  - 使用本地 LLM（Ollama）分別產生文章摘要（如有擷取成功）與討論摘要。
  - 將 10 則文章的摘要彙整為單一 HTML 電子郵件，並於手動 CLI 觸發時透過 Nodemailer 寄出。
  - 作為 agent-driven 開發、TypeScript、Node.js v22、API 整合、本地 LLM 與設定管理的學習平台，並善用「bmad-boilerplate」結構與工具。
- **可衡量成果：**
  - 工具能於多次手動 CLI 觸發下，完整執行（擷取、嘗試擷取文章、摘要、寄信）且不中斷。
  - 產生的電子郵件摘要始終包含 10 則文章的結果，正確顯示連結、討論摘要，以及如有擷取成功的文章摘要。
  - 擷取文章失敗時，錯誤會被記錄，並僅以討論摘要繼續流程，不會終止腳本。
- **成功標準：**
  - 連續 3 次手動 CLI 觸發能成功執行端到端流程。
  - 成功寄送並收到包含所有 10 則文章摘要（文章摘要視擷取結果而定）的電子郵件。
  - 擷取失敗能正確記錄錯誤，且不影響整體流程進行。
- **關鍵績效指標（KPI）：**
  - 成功執行次數 / 總執行次數（目標：MVP 測試 100%）
  - 含文章摘要的文章數 / 總文章數（衡量擷取成效）
  - 含討論摘要的文章數 / 總文章數（目標：100%）
  * 人工質性檢查：摘要內容的相關性與連貫性。

## 範圍與需求（MVP／目前版本）

### 功能性需求（高層級）

- **HN 文章擷取：** 從 Algolia HN Search API 取得前 10 則文章的 ID 與基本資料（標題、URL、HN 連結）。
- **HN 留言擷取：** 針對每篇文章，從 Algolia HN Search API 擷取留言，數量上限由 `.env` 設定變數（`MAX_COMMENTS_PER_STORY`，預設 50）決定。
- **文章內容擷取：** 嘗試以基本方式（如 Node.js 原生 fetch，或選用 `article-extractor` 等簡單函式庫）擷取外部文章 HTML 並抽取主文內容。
- **擷取失敗處理：** 若擷取失敗，需記錄錯誤，並僅產生該文章的討論摘要。
- **LLM 摘要生成：**
  - 使用設定的本地 LLM（Ollama endpoint）針對成功擷取的文章內文產生「文章摘要」。
  - 針對擷取到的留言內容產生「討論摘要」。
  - 初始提示詞（佔位，後續於 Epic 微調）：
    - _文章提示詞：_「請摘要下列文章內容之重點：{Article Text}」
    - _討論提示詞：_「請摘要下列 Hacker News 留言之主題、觀點與關鍵洞見：{Comment Texts}」
- **摘要格式化：** 將 10 則文章的結果合併成單一 HTML 電子郵件。每則文章需包含：標題、HN 連結、文章連結、文章摘要（如有）、討論摘要。
- **電子郵件發送：** 使用 Nodemailer 依 `.env` 定義的收件人清單寄送格式化後的 HTML 郵件。寄件憑證亦存於 `.env`。
- **主執行觸發：** 以手動命令列（CLI）觸發 _完整已實作流程_，使用樣板標準腳本（`npm run dev`、編譯後 `npm start`）。每個功能 epic 應將自身能力整合進主流程。
- **設定管理：** 透過 `.env` 檔管理外部參數（如 Algolia API 資訊（如需）、LLM endpoint、`MAX_COMMENTS_PER_STORY`、Nodemailer 憑證、收件人清單、輸出目錄路徑），依據 `.env.example` 為基礎。
- **漸進式記錄與資料儲存：**
  - 於流程各步驟以主控台記錄重要流程與錯誤。
  - 將中間資料（擷取的文章／留言、擷取內文、生成摘要）儲存於本地檔案，並依日期建立可設定的目錄結構（如 `./output/YYYY-MM-DD/`）。
  - 此儲存應於相關功能 epic（資料擷取、擷取、摘要）中漸進實作。
- **階段測試工具：**
  - 提供獨立工具腳本或 CLI 指令，允許單獨測試各流程階段（如擷取 HN 資料、擷取網址、文字摘要、寄送郵件）。
  - 工具應支援以本地儲存檔案作為輸入（如以網址檔案測試擷取、以文字檔案測試摘要），方便開發及除錯。

### 非功能性需求（NFRs）

- **效能：** MVP 以功能完整為主，不特別追求速度。於一般開發機本地 LLM 下應能於合理時間內完成（如 < 5 分鐘）。無特定回應時間要求。
- **可擴展性：** 僅設計單一使用者本地執行，MVP 無需考量擴展。
- **可靠性／可用性：**
  - 腳本需能優雅處理文章擷取失敗（記錄並繼續）。
  - API 呼叫需有基礎錯誤處理（如記錄網路錯誤）。
  - 本地 LLM 互動可能失敗，MVP 僅需基本錯誤記錄。
  - 無需自動重試或生產等級錯誤處理。
- **安全性：**
  - 郵件憑證必須透過 `.env` 儲存，且不得提交至版本控制（依樣板 `.gitignore`）。
  - 本地 MVP 無其他特定安全需求。
- **可維護性：**
  - 程式碼應為良好結構之 TypeScript。
  - 必須遵循「bmad-boilerplate」內設定的 lint（ESLint）與格式化（Prettier）規則。請使用 `npm run lint` 與 `npm run format`。
  - 需具備模組化設計，以便日後更換 LLM 供應商及支援階段測試。
- **可用性／無障礙性：** 無需考量（開發者用 CLI 工具）。
- **其他限制：**
  - 必須使用 TypeScript 與 Node.js v22。
  - 必須於開發者本機執行。
  - 必須使用 Node.js v22 原生 `Workspace` API 進行 HTTP 請求。
  - 必須使用 Algolia HN Search API 擷取 HN 資料。
  - 必須透過可設定 HTTP endpoint 連接本地 Ollama 實例。
  - 必須使用 Nodemailer 寄送郵件。
  - 必須依 `.env.example` 以 `.env` 進行設定管理。
  - 必須以本地檔案系統進行記錄及中間資料儲存。請確保 output/log 目錄已納入 gitignore。
  - 著重於學習／展示用的功能性流程。

### 使用者體驗（UX）需求（高層級）

- 主要 UX 目標為提供節省時間的摘要。
- 對於開發者用戶，CLI 操作應簡單明瞭：可直接以樣板標準腳本（如 `npm run dev` 或 `npm start`）觸發完整流程。
- CLI 執行過程中，建議以主控台訊息回饋進度（如「正在擷取文章...」、「正在摘要第 X/10 篇...」、「正在寄送郵件...」）。
- 測試各階段的 CLI 指令／腳本，應有清楚的輸入輸出機制。

### 整合需求（高層級）

- **Algolia HN Search API：** 擷取熱門文章與留言。需理解 API 結構與查詢參數。
- **Ollama 服務：** 透過 API endpoint 傳送文章或留言文字並取得摘要。endpoint URL 必須可設定。
- **SMTP 服務（透過 Nodemailer）：** 寄送最終摘要郵件。需於 `.env` 設定有效 SMTP 憑證與收件人清單。

### 測試需求（高層級）

- MVP 成功與否需仰賴手動端到端測試，確認流程能順利執行並產生有效郵件。
- 鼓勵使用 **樣板已設置的 Jest 框架** 進行單元／整合測試，並聚焦於核心流程組件。請執行 `npm run test`。
- **必須提供各階段專用測試工具（如功能性需求所述）**，以支援開發與驗證各流程組件。

## Epic 概覽（MVP／目前版本）

_(修正版提案)_

- **Epic 1：專案初始化與核心設置** - 目標：以「bmad-boilerplate」初始化專案、管理相依套件、設置 `.env` 與設定載入、建立基本 CLI 進入點、設置基礎記錄與輸出目錄結構。
- **Epic 2：HN 資料擷取與儲存** - 目標：實作從 Algolia HN API 擷取前 10 則文章及留言（依上限），並將原始資料本地儲存。實作擷取階段測試工具。
- **Epic 3：文章擷取與儲存** - 目標：實作盡力擷取／抽取文章內容，優雅處理失敗，並本地儲存擷取文字。實作擷取階段測試工具。
- **Epic 4：LLM 摘要與儲存** - 目標：整合 Ollama，從儲存資料產生文章／討論摘要並本地儲存。實作摘要階段測試工具。
- **Epic 5：摘要彙整與郵件寄送** - 目標：以儲存資料格式化 HTML 郵件並透過 Nodemailer 寄送。實作郵件階段測試工具（含 dry-run 選項）。

## 主要參考文件

- `docs/project-brief.md`
- `docs/prd.md`（本文件）
- `docs/architecture.md`（由架構師建立）
- `docs/epic1.md`、`docs/epic2.md`、...（待建立）
- `docs/tech-stack.md`（部份由樣板定義，最終由架構師確認）
- `docs/api-reference.md`（如需 Algolia/Ollama 細節）
- `docs/testing-strategy.md`（選填，MVP 可低優先，Jest 設定已提供）

## MVP 後／未來增強項目

- 進階擷取技術（處理 JavaScript、反爬蟲機制）。
- 處理所有留言（可考慮 MapReduce 摘要）。
- 自動化排程（如使用 cron）。
- 整合資料庫以儲存結果或追蹤。
- 雲端部署與網頁前端。
- 使用者管理（註冊、偏好設定）。
- 生產等級錯誤處理、監控與郵件投遞追蹤。
- 微調 LLM 提示詞或模型。
- 進階 API 呼叫或擷取重試邏輯。
- 雲端 LLM 整合。

## 變更紀錄

| 變更內容                | 日期        | 版本     | 說明                                   | 作者   |
| ----------------------- | ---------- | ------- | -------------------------------------- | ------ |
| 修正 Epic 與測試需求    | 2025-05-04 | 0.3     | 移除 Epic 6，新增階段測試需求           | 2-pm   |
| 新增樣板                | 2025-05-04 | 0.2     | 反映樣板使用狀態                       | 2-pm   |
| 初稿                    | 2025-05-04 | 0.1     | 根據簡報撰寫第一版                      | 2-pm   |

## 初始架構師提示

### 技術基礎設施

- **起始專案／樣板：** **強制：必須使用提供的「bmad-boilerplate」。** 內含 TypeScript 設定、Node.js v22 相容、Jest、ESLint、Prettier、`ts-node`、`.env` 處理（依 `.env.example`）、標準腳本（`dev`、`build`、`test`、`lint`、`format`）。
- **託管／雲端供應商：** MVP 僅限本地執行。無雲端部署。
- **前端平台：** 無需考慮（CLI 工具）。
- **後端平台：** Node.js v22 + TypeScript（依樣板）。不強制特定 Node.js 框架，但結構需支援模組化並符合樣板設計。
- **資料庫需求：** 無。僅以本地檔案系統儲存中間資料與記錄。結構待定（如 `./output/YYYY-MM-DD/`）。請確保輸出目錄可由 `.env` 設定且已納入 gitignore。

### 技術限制

- 必須遵循「bmad-boilerplate」的結構與工具。
- 必須使用 Node.js v22 原生 `Workspace` 進行 HTTP 請求。
- 必須使用 Algolia HN Search API 擷取 HN 資料。
- 必須透過可設定 HTTP endpoint 整合本地 Ollama 實例。架構應可日後替換其他 LLM API。
- 必須使用 Nodemailer 寄送郵件。
- 設定（LLM endpoint、郵件憑證、收件人、`MAX_COMMENTS_PER_STORY`、輸出目錄路徑）必須以 `.env` 管理，並依 `.env.example`。
- 文章擷取需以簡單、盡力而為方式，失敗時需優雅處理且不影響主流程。
- 中間資料需逐步本地儲存。
- 程式碼必須遵循樣板內的 ESLint 與 Prettier 設定。

### 部署考量

- 僅允許以 CLI 手動觸發（`npm run dev` 或 `npm start`）。
- MVP 無需 CI/CD。
- 僅有單一環境：本地開發機。

### 本地開發與測試需求

- 全部應用程式於本地執行。
- 主 CLI 指令（`npm run dev`／`start`）應執行 _完整已實作流程_。
- **必須提供獨立工具腳本／指令**，以測試各流程階段（擷取、擷取、摘要、寄信），並支援本地檔案輸入／輸出。架構應助於建立這些階段執行器（如：`npm run stage:fetch`、`npm run stage:scrape -- --inputFile <path>`、`npm run stage:summarize -- --inputFile <path>`、`npm run stage:email -- --inputFile <path> [--dry-run]`）。
- 樣板已提供 `npm run test` 執行自動化單元／整合測試（Jest）。
- 樣板已提供 `npm run lint` 與 `npm run format` 進行程式碼品質檢查。
- 需有基本主控台記錄。檔案記錄可由架構師視情況考慮。
- 各模組（API client、擷取器、摘要器、郵件寄送）可測試性很重要，應善用 Jest 與階段測試工具。

### 其他技術考量

- **模組化：** 各元件（HN client、擷取器、LLM client、郵件寄送）需有明確介面，便於日後更換 LLM 供應商與獨立階段測試。
- **錯誤處理：** 著重於擷取失敗的健壯處理，以及 API／網路錯誤的基本處理。請於樣板結構內實作。記錄需明確標示錯誤。
- **資源管理：** 與 LLM 互動時需留意本地資源消耗，雖 MVP 不以最佳化為目標。
- **相依管理：** 僅新增必要生產相依（如 `nodemailer`、`article-extractor`、日期處理或檔案系統函式庫等），並維持最小化。
- **設定載入：** 應於應用啟動初期實作健全的 `.env` 設定載入與驗證。
