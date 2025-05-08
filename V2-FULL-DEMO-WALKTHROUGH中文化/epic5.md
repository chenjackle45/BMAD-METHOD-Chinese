# Epic 5: 摘要組裝與電子郵件發送

**目標：** 將從本地文件中收集的故事數據和摘要組裝成可讀的 HTML 電子郵件摘要，並使用配置的憑據通過 Nodemailer 發送電子郵件。實現一個帶有乾跑選項的電子郵件階段測試工具。

## 故事列表

### 故事 5.1: 實現電子郵件內容組裝器

- **使用者故事 / 目標：** 作為一名開發者，我希望有一個模組可以從指定目錄中讀取持久化的故事 Metadata（`_data.json`）和摘要（`_summary.json`），整合渲染電子郵件摘要所需的資訊。
- **詳細需求：**
  - 建立一個新模組：`src/email/contentAssembler.ts`。
  - 定義一個 TypeScript 類型/介面 `DigestData`，表示電子郵件模板每個故事所需的數據：`{ storyId: string, title: string, hnUrl: string, articleUrl: string | null, articleSummary: string | null, discussionSummary: string | null }`。
  - 實現一個非同步函式 `assembleDigestData(dateDirPath: string): Promise<DigestData[]>`。
  - 該函式應：
    - 使用 Node.js 的 `fs` 讀取 `dateDirPath` 的內容。
    - 識別所有符合模式 `{storyId}_data.json` 的文件。
    - 對於找到的每個 `storyId`：
      - 讀取並解析 `{storyId}_data.json` 文件。提取 `title`、`hnUrl` 和 `url`（作為 `articleUrl`）。妥善處理潛在的文件讀取/解析錯誤（記錄並跳過該故事）。
      - 嘗試讀取並解析對應的 `{storyId}_summary.json` 文件。妥善處理文件未找到或解析錯誤（將 `articleSummary` 和 `discussionSummary` 視為 `null`）。
      - 為該故事構建一個 `DigestData` 物件，包括提取的 Metadata 和摘要（或 null）。
    - 收集所有成功構建的 `DigestData` 物件到一個陣列中。
    - 返回該陣列。如果所有前置階段成功，理想情況下應包含 10 個項目。
  - 使用記錄器記錄進度（例如，"從目錄組裝摘要數據..."，"處理故事 {storyId}..."）以及文件處理過程中遇到的任何錯誤。
- **驗收標準（ACs）：**
  - AC1：`contentAssembler.ts` 模組存在並導出 `assembleDigestData` 和 `DigestData` 類型。
  - AC2：`assembleDigestData` 能正確讀取提供的目錄路徑中的 `_data.json` 文件。
  - AC3：它嘗試讀取對應的 `_summary.json` 文件，並正確處理摘要文件可能缺失或無法解析的情況（導致該故事的摘要為 null）。
  - AC4：該函式返回一個 Promise，解析為包含從文件中提取數據的 `DigestData` 物件陣列。
  - AC5：文件讀取或 JSON 解析過程中的錯誤被記錄，且函式返回成功處理的故事數據。

---

### 故事 5.2: 建立 HTML 電子郵件模板與渲染器

- **使用者故事 / 目標：** 作為一名開發者，我希望有一個基本的 HTML 電子郵件模板和一個函式，能使用組裝的摘要數據渲染它，生成電子郵件正文的最終 HTML 內容。
- **詳細需求：**
  - 定義 HTML 結構。這可以通過函式內的模板字面量完成，或者使用簡單的模板文件（例如，`src/email/templates/digestTemplate.html`）和 `fs.readFileSync`。對於 MVP，模板字面量更簡單。
  - 建立一個函式 `renderDigestHtml(data: DigestData[], digestDate: string): string`（例如，在 `src/email/contentAssembler.ts` 或新的 `templater.ts` 中）。
  - 該函式應生成一個 HTML 字串，包含：
    - 主體中的合適標題（例如，`<h1>Hacker News Top 10 Summaries for ${digestDate}</h1>`）。
    - 一個迴圈遍歷 `data` 陣列。
    - 對於 `data` 中的每個故事：
      - 顯示 `<h2><a href="${story.articleUrl || story.hnUrl}">${story.title}</a></h2>`。
      - 顯示 `<p><a href="${story.hnUrl}">View HN Discussion</a></p>`。
      - 僅當 `story.articleSummary` 不為 null/空時，條件性地顯示 `<h3>Article Summary</h3><p>${story.articleSummary}</p>`。
      - 僅當 `story.discussionSummary` 不為 null/空時，條件性地顯示 `<h3>Discussion Summary</h3><p>${story.discussionSummary}</p>`。
      - 包括分隔符（例如，`<hr style="margin-top: 20px; margin-bottom: 20px;">`）。
  - 使用基本的內聯 CSS 進行最小樣式設計（邊距等），以確保可讀性。避免複雜的佈局。
  - 返回完整的 HTML 文檔作為字串。
- **驗收標準（ACs）：**
  - AC1：存在函式 `renderDigestHtml`，接受摘要數據陣列和日期字串。
  - AC2：該函式返回單一完整的 HTML 字串。
  - AC3：生成的 HTML 包含帶有日期的標題，並正確迭代故事數據。
  - AC4：對於每個故事，HTML 顯示鏈接的標題、HN 鏈接，並條件性地顯示文章和討論摘要及其標題。
  - AC5：使用基本的分隔符和邊距以確保可讀性。HTML 簡單且可能在大多數電子郵件客戶端中合理渲染。

---

### 故事 5.3: 實現 Nodemailer 電子郵件發送器

- **使用者故事 / 目標：** 作為一名開發者，我希望有一個模組可以使用 Nodemailer 發送生成的 HTML 電子郵件，並使用安全存儲在環境文件中的憑據進行配置。
- **詳細需求：**
  - 新增 Nodemailer 相依套件：`npm install nodemailer @types/nodemailer --save-prod`。
  - 在 `.env.example`（以及本地 `.env`）中新增所需的配置變數：`EMAIL_HOST`、`EMAIL_PORT`（例如，587）、`EMAIL_SECURE`（例如，對於 587 的 STARTTLS 為 `false`，對於 465 為 `true`）、`EMAIL_USER`、`EMAIL_PASS`、`EMAIL_FROM`（例如，`"Your Name <you@example.com>"`）、`EMAIL_RECIPIENTS`（逗號分隔的列表）。
  - 建立一個新模組：`src/email/emailSender.ts`。
  - 實現一個非同步函式 `sendDigestEmail(subject: string, htmlContent: string): Promise<boolean>`。
  - 在函式內：
    - 從配置模組加載 `EMAIL_*` 變數。
    - 使用 `nodemailer.createTransport` 和加載的配置（host、port、secure 標誌、auth: { user, pass }）建立一個 Nodemailer 傳輸器。
    - 使用 `transporter.verify()` 驗證傳輸器配置（可選但推薦）。記錄驗證成功/失敗。
    - 將 `EMAIL_RECIPIENTS` 字串解析為適合 `to` 欄位的陣列或逗號分隔字串。
    - 定義 `mailOptions`：`{ from: EMAIL_FROM, to: parsedRecipients, subject: subject, html: htmlContent }`。
    - 呼叫 `await transporter.sendMail(mailOptions)`。
    - 如果 `sendMail` 成功，記錄成功消息，包括結果中的 `messageId`。返回 `true`。
    - 如果 `sendMail` 失敗（拋出錯誤），使用記錄器記錄錯誤。返回 `false`。
- **驗收標準（ACs）：**
  - AC1：新增 `nodemailer` 和 `@types/nodemailer` 相依套件。
  - AC2：`EMAIL_*` 變數定義在 `.env.example` 並從配置中加載。
  - AC3：`emailSender.ts` 模組存在並導出 `sendDigestEmail`。
  - AC4：`sendDigestEmail` 使用來自 `.env` 的配置正確建立 Nodemailer 傳輸器。嘗試進行傳輸器驗證（可選 AC）。
  - AC5：`to` 欄位根據 `EMAIL_RECIPIENTS` 正確填充。
  - AC6：`transporter.sendMail` 使用正確的 `from`、`to`、`subject` 和 `html` 選項進行呼叫。
  - AC7：電子郵件發送成功（包括消息 ID）或失敗被清楚記錄。
  - AC8：函式在成功發送時返回 `true`，否則返回 `false`。

---

### 故事 5.4: 將電子郵件組裝與發送整合到主工作流程

- **使用者故事 / 目標：** 作為一名開發者，我希望主應用程式工作流程（`src/index.ts`）能協調最後步驟：組裝摘要數據、渲染 HTML，並在所有前置階段完成後觸發電子郵件發送。
- **詳細需求：**
  - 修改 `src/index.ts` 中的主執行流程。
  - 匯入 `assembleDigestData`、`renderDigestHtml`、`sendDigestEmail`。
  - 在主迴圈（抓取、抓取、摘要生成和持久化故事完成後）完成後執行以下步驟：
    - 記錄 "開始最終摘要組裝與電子郵件發送..."。
    - 確定當前日期標記的輸出目錄的路徑。
    - 呼叫 `const digestData = await assembleDigestData(dateDirPath)`。
    - 檢查 `digestData` 陣列是否非空。
      - 如果是：
        - 獲取當前日期字串（例如，'YYYY-MM-DD'）。
        - `const htmlContent = renderDigestHtml(digestData, currentDate)`。
        - `const subject = \`BMad Hacker Daily Digest - ${currentDate}\``。
        - `const emailSent = await sendDigestEmail(subject, htmlContent)`。
        - 根據 `emailSent` 記錄最終結果（"摘要電子郵件發送成功。" 或 "摘要電子郵件發送失敗。"）。
      - 如果否（`digestData` 為空或組裝失敗）：
        - 記錄錯誤："摘要數據組裝失敗或未找到數據。跳過電子郵件。"。
    - 記錄 "BMad Hacker Daily Digest 流程完成。"。
- **驗收標準（ACs）：**
  - AC1：執行 `npm run dev` 時，執行所有階段（Epic 1-4），然後進行電子郵件組裝與發送。
  - AC2：`assembleDigestData` 在其他處理完成後正確呼叫，並使用輸出目錄路徑。
  - AC3：如果數據已組裝，`renderDigestHtml` 和 `sendDigestEmail` 使用正確的數據、主題和 HTML 進行呼叫。
  - AC4：電子郵件發送步驟的最終成功或失敗被記錄。
  - AC5：如果 `assembleDigestData` 返回無數據，則跳過電子郵件發送，並記錄適當消息。
  - AC6：應用程式記錄最終完成消息。

---

### 故事 5.5: 為電子郵件實現階段測試工具

- **使用者故事 / 目標：** 作為一名開發者，我希望有一個單獨的腳本/命令來測試電子郵件組裝、渲染和發送邏輯，使用持久化的本地數據，包括一個關鍵的 `--dry-run` 選項，以防止測試期間意外發送電子郵件。
- **詳細需求：**
  - 新增 `yargs` 相依套件以進行參數解析：`npm install yargs @types/yargs --save-dev`。
  - 建立一個新的獨立腳本文件：`src/stages/send_digest.ts`。
  - 匯入必要的模組：`fs`、`path`、`logger`、`config`、`assembleDigestData`、`renderDigestHtml`、`sendDigestEmail`、`yargs`。
  - 使用 `yargs` 解析命令列參數，特別是查找 `--dry-run` 布林標誌（默認為 `false`）。允許指定日期標記目錄的可選參數，否則默認為當前日期。
  - 該腳本應：
    - 初始化記錄器，載入配置。
    - 確定目標日期標記的目錄路徑（來自參數或默認值）。記錄目標目錄。
    - 呼叫 `await assembleDigestData(dateDirPath)`。
    - 如果數據已組裝且非空：
      - 確定主題/標題的日期字串。
      - 呼叫 `renderDigestHtml(digestData, dateString)` 獲取 HTML。
      - 構建主題字串。
      - 檢查 `dryRun` 標誌：
        - 如果為 `true`：記錄 "已啟用乾跑模式。跳過實際電子郵件發送。"。記錄主題。將 `htmlContent` 保存到目標目錄中的文件（例如，`_digest_preview.html`）。記錄預覽文件已保存。
        - 如果為 `false`：記錄 "實時運行：嘗試發送電子郵件..."。呼叫 `await sendDigestEmail(subject, htmlContent)`。根據返回值記錄成功/失敗。
    - 如果數據組裝失敗或為空，記錄錯誤。
  - 在 `package.json` 中新增腳本：`"stage:email": "ts-node src/stages/send_digest.ts --"`。`--` 允許傳遞參數，如 `--dry-run`。
- **驗收標準（ACs）：**
  - AC1：文件 `src/stages/send_digest.ts` 存在。新增 `yargs` 相依套件。
  - AC2：腳本 `stage:email` 定義在 `package.json` 中，允許參數。
  - AC3：執行 `npm run stage:email -- --dry-run` 讀取本地數據，渲染 HTML，記錄意圖，將 `_digest_preview.html` 本地保存，且*不*呼叫 `sendDigestEmail`。
  - AC4：執行 `npm run stage:email`（無 `--dry-run`）讀取本地數據，渲染 HTML，並*呼叫* `sendDigestEmail`，記錄結果。
  - AC5：腳本正確識別並執行 `--dry-run` 標誌。
  - AC6：日誌清楚區分乾跑和實時運行，並報告成功/失敗。
  - AC7：腳本僅使用本地文件和電子郵件配置/服務運行；不調用先前的管道階段（Algolia、抓取、Ollama）。

## 變更日誌

| 變更     | 日期       | 版本 | 描述              | 作者 |
| -------- | ---------- | ---- | ----------------- | ---- |
| 初始草案 | 2025-05-04 | 0.1  | Epic 5 的初始草案 | 2-pm |
