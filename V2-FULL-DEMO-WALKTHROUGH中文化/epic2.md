# Epic 2: HN 資料擷取與持久化

**目標：** 實作從 Algolia HN API 擷取前 10 篇文章及其評論（遵守限制），並將此原始資料持久化儲存到 Epic 1 中建立的日期標記輸出目錄中。實作一個階段測試工具以進行資料擷取。

## 故事列表

### 故事 2.1: 實作 Algolia HN API 客戶端

- **使用者故事 / 目標：** 作為一名開發者，我希望有一個專用的客戶端模組來與 Algolia Hacker News Search API 互動，以便將文章和評論的擷取邏輯封裝、可重複使用，並使用所需的原生 `Workspace` API。
- **詳細需求：**
  - 建立一個新模組：`src/clients/algoliaHNClient.ts`。
  - 在客戶端中實作一個非同步函式 `WorkspaceTopStories`：
    - 使用原生 `Workspace` 呼叫 Algolia HN Search API 的首頁文章端點（例如：`http://hn.algolia.com/api/v1/search?tags=front_page&hitsPerPage=10`）。如有需要，調整 `hitsPerPage` 以確保有 10 篇文章。
    - 解析 JSON 回應。
    - 擷取每篇文章所需的 Metadata：`objectID`（作為 `storyId`）、`title`、`url`（文章 URL）、`points`、`num_comments`。妥善處理可能缺失的 `url` 欄位（記錄警告，若需要 URL，稍後可能跳過該文章）。
    - 為每篇文章建構 `hnUrl`（例如：`https://news.ycombinator.com/item?id={storyId}`）。
    - 回傳結構化的文章物件陣列。
  - 在客戶端中實作另一個非同步函式 `WorkspaceCommentsForStory`：
    - 接受 `storyId` 和 `maxComments` 限制作為參數。
    - 使用原生 `Workspace` 呼叫特定文章的評論端點（例如：`http://hn.algolia.com/api/v1/search?tags=comment,story_{storyId}&hitsPerPage={maxComments}`）。
    - 解析 JSON 回應。
    - 擷取所需的評論資料：`objectID`（作為 `commentId`）、`comment_text`、`author`、`created_at`。
    - 過濾掉 `comment_text` 為 null 或空的評論。確保回傳的評論數量不超過 `maxComments`。
    - 回傳結構化的評論物件陣列。
  - 使用 `try...catch` 包裹 `Workspace` 呼叫，並檢查 `response.ok` 狀態以實作基本錯誤處理。使用 Epic 1 的記錄器工具記錄錯誤。
  - 定義 TypeScript 介面/型別以描述 API 回應的預期結構（文章、評論）以及客戶端函式回傳的資料（例如：`Story`、`Comment`）。
- **驗收標準（ACs）：**
  - AC1：模組 `src/clients/algoliaHNClient.ts` 存在，並匯出 `WorkspaceTopStories` 和 `WorkspaceCommentsForStory` 函式。
  - AC2：呼叫 `WorkspaceTopStories` 時，正確地向 Algolia 端點發出網路請求，並回傳一個 Promise，解析為包含指定 Metadata 的 10 個 `Story` 物件的陣列。
  - AC3：呼叫 `WorkspaceCommentsForStory` 並傳入有效的 `storyId` 和 `maxComments` 限制時，正確地向 Algolia 端點發出網路請求，並回傳一個 Promise，解析為最多包含 `maxComments` 條 `Comment` 物件的陣列，且過濾掉空評論。
  - AC4：兩個函式內部均使用原生 `Workspace` API。
  - AC5：網路錯誤或非成功的 API 回應（例如：狀態碼 4xx、5xx）會被捕獲並使用記錄器記錄。
  - AC6：相關的 TypeScript 型別（`Story`、`Comment` 等）已定義並在客戶端模組中使用。

---

### 故事 2.2: 將 HN 資料擷取整合到主工作流程中

- **使用者故事 / 目標：** 作為一名開發者，我希望將 HN 資料擷取邏輯整合到主應用程式工作流程中（`src/index.ts`），以便在完成 Epic 1 的設定後，執行應用程式時能擷取前 10 篇文章及其評論。
- **詳細需求：**
  - 修改 `src/index.ts` 中的主要執行流程（或由其呼叫的主要非同步函式）。
  - 匯入 `algoliaHNClient` 函式。
  - 匯入配置模組以存取 `MAX_COMMENTS_PER_STORY`。
  - 在 Epic 1 的設定步驟（配置載入、記錄器初始化、輸出目錄建立）完成後，呼叫 `WorkspaceTopStories()`。
  - 記錄擷取到的文章數量。
  - 遍歷擷取到的 `Story` 物件陣列。
  - 對於每個 `Story`，呼叫 `WorkspaceCommentsForStory()`，傳入 `story.storyId` 和配置的 `MAX_COMMENTS_PER_STORY`。
  - 將擷取到的評論儲存在對應的 `Story` 物件中（例如，新增一個 `comments: Comment[]` 屬性到 `Story` 物件）。
  - 使用記錄器工具記錄進度（例如："已擷取 10 篇文章。"、"正在為文章 {storyId} 擷取最多 X 條評論..."）。
- **驗收標準（ACs）：**
  - AC1：執行 `npm run dev` 時，會執行 Epic 1 的設定步驟，接著擷取文章，然後為每篇文章擷取評論。
  - AC2：記錄清楚顯示擷取文章的開始與成功完成，以及為每篇文章擷取評論的開始。
  - AC3：配置的 `MAX_COMMENTS_PER_STORY` 值從配置中讀取，並用於呼叫 `WorkspaceCommentsForStory`。
  - AC4：成功執行後，記憶體中的文章物件包含一個嵌套的評論物件陣列。（可透過除錯器或臨時記錄進行驗證）。

---

### 故事 2.3: 將擷取的 HN 資料持久化儲存到本地

- **使用者故事 / 目標：** 作為一名開發者，我希望將擷取的 HN 文章（包括其評論）儲存為 JSON 檔案到日期標記的輸出目錄中，以便後續的管道階段和除錯使用。
- **詳細需求：**
  - 定義輸出檔案內容的一致 JSON 結構。例如：`{ storyId: "...", title: "...", url: "...", hnUrl: "...", points: ..., fetchedAt: "ISO_TIMESTAMP", comments: [{ commentId: "...", text: "...", author: "...", createdAt: "ISO_TIMESTAMP", ... }, ...] }`。包含資料擷取時的時間戳。
  - 匯入 Node.js 的 `fs`（特別是 `fs.writeFileSync`）和 `path` 模組。
  - 在主工作流程（`src/index.ts`）中，在遍歷文章的迴圈內（在評論已擷取並新增到文章物件後）：
    - 獲取日期標記輸出目錄的完整路徑（在 Epic 1 中確定）。
    - 為文章資料構建檔案名稱：`{storyId}_data.json`。
    - 使用 `path.join()` 構建完整檔案路徑。
    - 使用 `JSON.stringify(storyObject, null, 2)` 將完整的文章物件（包括評論和擷取時間戳）序列化為 JSON 字串，便於閱讀。
    - 使用 `fs.writeFileSync()` 將 JSON 字串寫入檔案。使用 `try...catch` 區塊進行錯誤處理。
  - 使用記錄器記錄每篇文章資料檔案的成功儲存或在寫入檔案時遇到的任何錯誤。
- **驗收標準（ACs）：**
  - AC1：執行 `npm run dev` 後，日期標記輸出目錄（例如：`./output/YYYY-MM-DD/`）包含正好 10 個名為 `{storyId}_data.json` 的檔案。
  - AC2：每個 JSON 檔案包含有效的 JSON，表示單個文章物件，包括其 Metadata、擷取時間戳，以及其擷取的評論陣列，符合定義的結構。
  - AC3：每個檔案的 `comments` 陣列中的評論數量不超過 `MAX_COMMENTS_PER_STORY`。
  - AC4：記錄顯示已嘗試為每篇文章儲存資料到檔案，並報告成功或具體的檔案寫入錯誤。

---

### 故事 2.4: 為 HN 擷取實作階段測試工具

- **使用者故事 / 目標：** 作為一名開發者，我希望有一個獨立的可執行腳本，僅執行 HN 資料擷取與持久化，以便獨立於完整管道測試和觸發此階段。
- **詳細需求：**
  - 建立一個新的獨立腳本檔案：`src/stages/fetch_hn_data.ts`。
  - 此腳本應執行此階段所需的基本設定：初始化記錄器、載入配置（`.env`）、確定並建立輸出目錄（重用或複製 Epic 1 / `src/index.ts` 的邏輯）。
  - 腳本應執行核心邏輯：透過 `algoliaHNClient.fetchTopStories` 擷取文章，透過 `algoliaHNClient.fetchCommentsForStory` 擷取評論（使用載入的配置限制），並使用 `fs.writeFileSync` 持久化結果為 JSON 檔案（複製故事 2.3 的邏輯）。
  - 腳本應使用記錄器記錄其進度。
  - 在 `package.json` 的 `"scripts"` 中新增一個腳本命令：`"stage:fetch": "ts-node src/stages/fetch_hn_data.ts"`。
- **驗收標準（ACs）：**
  - AC1：檔案 `src/stages/fetch_hn_data.ts` 存在。
  - AC2：腳本 `stage:fetch` 已定義於 `package.json` 的 `"scripts"` 區段中。
  - AC3：執行 `npm run stage:fetch` 成功執行，僅執行設定、擷取和持久化步驟。
  - AC4：執行 `npm run stage:fetch` 會在正確的日期標記輸出目錄中建立與執行主 `npm run dev` 指令相同的 10 個 `{storyId}_data.json` 檔案（以當前開發狀態為準）。
  - AC5：`npm run stage:fetch` 產生的記錄僅反映擷取和持久化步驟，而非後續管道階段。

## 變更記錄

| 變更 | 日期       | 版本 | 描述        | 作者 |
| ---- | ---------- | ---- | ----------- | ---- |
| 初稿 | 2025-05-04 | 0.1  | Epic 2 初稿 | 2-pm |
