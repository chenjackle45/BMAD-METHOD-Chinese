# Epic 1 檔案

# Epic 1：專案初始化與核心設置

**目標：** 使用 "bmad-boilerplate" 初始化專案、管理相依套件、設置 `.env` 與組態載入、建立基本 CLI 進入點、設置基本日誌與輸出目錄結構。這提供所有後續開發工作的基礎設置。

## 使用者故事清單

### Story 1.1：從樣板初始化專案

- **使用者故事 / 目標：** 作為開發者，我希望使用 `bmad-boilerplate` 設置初始專案結構，以便擁有標準工具（TS, Jest, ESLint, Prettier）、組態和腳本。
- **詳細需求：**
  - 將 `bmad-boilerplate` 的內容複製或 clone 到新專案的根目錄。
  - 在專案根目錄初始化 git 儲存庫（如果 clone 時尚未建立）。
  - 確認樣板中的 `.gitignore` 檔案存在。
  - 執行 `npm install` 下載並安裝樣板 `package.json` 所指定的所有 `devDependencies`。
  - 驗證樣板的核心腳本（`lint`, `format`, `test`, `build`）在初始程式碼庫上能正確執行且無錯誤。
- **驗收標準（ACs）：**
  - AC1：專案目錄包含來自 `bmad-boilerplate` 的檔案與結構。
  - AC2：存在 `node_modules` 目錄且包含對應 `devDependencies` 的套件。
  - AC3：`npm run lint` 指令可順利執行且無任何 lint 錯誤。
  - AC4：`npm run format` 指令可順利執行，並依 Prettier 規則自動修正格式。再次執行時不應有變更。
  - AC5：`npm run test` 指令可成功執行 Jest（此階段出現 "no tests found" 可接受）。
  - AC6：`npm run build` 指令可順利執行，並產生包含編譯後 JavaScript 的 `dist` 目錄。
  - AC7：`.gitignore` 檔案存在，且包含 `node_modules/`、`.env`、`dist/` 等樣板所指定的條目。

---

### Story 1.2: 設定環境配置

- **使用者故事 / 目標：** 作為開發者，我希望使用 `.env` 檔案建立環境配置機制，以便能在版本控制之外管理秘密和設定（如輸出路徑），遵循樣板慣例。
- **詳細需求：**
  - 增加一個生產相依套件以載入 `.env` 檔案（例如 `dotenv`）。執行 `npm install dotenv --save-prod`（或類似的庫）。
  - 確認 `.env.example` 檔案存在（來自樣板）。
  - 在 `.env.example` 中新增初始配置變數 `OUTPUT_DIR_PATH=./output`。
  - 通過複製 `.env.example` 在本地創建 `.env` 檔案。如果需要，填充 `OUTPUT_DIR_PATH`（可以保持默認）。
  - 實作一個工具模組（例如 `src/config.ts`），在應用啟動時從 `.env` 檔案載入環境變數。
  - 該工具應導出載入的配置值（最初僅 `OUTPUT_DIR_PATH`）。
  - 確保 `.env` 檔案列在 `.gitignore` 中且不被提交。
- **驗收標準（ACs）：**
  - AC1：選擇的 `.env` 庫（例如 `dotenv`）在 `package.json` 的 `dependencies` 中列出，並更新 `package-lock.json`。
  - AC2：`.env.example` 檔案存在，並被 git 追蹤，且包含行 `OUTPUT_DIR_PATH=./output`。
  - AC3：`.env` 檔案在本地存在，但不被 git 追蹤。
  - AC4：配置模組（`src/config.ts` 或類似）存在，並在應用啟動時成功載入 `.env` 中的 `OUTPUT_DIR_PATH` 值。
  - AC5：載入的 `OUTPUT_DIR_PATH` 值在應用程式代碼中可訪問。

---

### Story 1.3: 實作基本 CLI 進入點與執行

- **使用者故事 / 目標：** 作為開發者，我希望有一個基本的 `src/index.ts` 進入點，可以通過樣板的 `dev` 和 `start` 腳本執行，為應用邏輯提供一個可運行的基礎。
- **詳細需求：**
  - 在 `src/index.ts` 創建主要應用進入點檔案。
  - 在 `src/index.ts` 中實作最小代碼以：
    - 導入配置載入機制（來自 Story 1.2）。
    - 在控制台記錄簡單的啟動消息（例如 "BMad Hacker Daily Digest - Starting Up..."）。
    - （可選）記錄載入的 `OUTPUT_DIR_PATH` 以驗證配置載入。
  - 確認使用樣板腳本執行。
- **驗收標準（ACs）：**
  - AC1：`src/index.ts` 檔案存在。
  - AC2：執行 `npm run dev` 透過 `ts-node` 執行 `src/index.ts` 並在控制台記錄啟動消息。
  - AC3：執行 `npm run build` 成功編譯 `src/index.ts`（及任何導入）到 `dist` 目錄。
  - AC4：執行 `npm start`（在成功編譯後）執行來自 `dist` 的編譯代碼並在控制台記錄啟動消息。

---

### Story 1.4: 設定基本日誌與輸出目錄

- **使用者故事 / 目標：** 作為開發者，我希望有一個基本的控制台日誌機制和動態創建日期標記的輸出目錄，以便應用能提供執行反饋並為後續史詩中的數據工件存儲做準備。
- **詳細需求：**
  - 實作一個簡單、可重用的日誌工具模組（例如 `src/logger.ts`）。最初可以包裝 `console.log`、`console.warn`、`console.error`。
  - 重構 `src/index.ts` 以使用此 `logger` 進行啟動消息的記錄。
  - 在 `src/index.ts`（或由其調用的設置函數）中：
    - 從配置中檢索 `OUTPUT_DIR_PATH`（在 Story 1.2 中載入）。
    - 確定當前日期的 'YYYY-MM-DD' 格式。
    - 構建日期標記子目錄的完整路徑（例如 `${OUTPUT_DIR_PATH}/YYYY-MM-DD`）。
    - 檢查基礎輸出目錄是否存在；如果不存在，則創建它。
    - 檢查日期標記子目錄是否存在；如果不存在，則遞歸創建它。使用 Node.js `fs` 模組（例如 `fs.mkdirSync(path, { recursive: true })`）。
    - 使用日誌記錄（使用 logger）當前運行所使用的輸出目錄的完整路徑（例如 "Output directory for this run: ./output/2025-05-04"）。
- **驗收標準（ACs）：**
  - AC1：日誌工具模組（`src/logger.ts` 或類似）存在並在 `src/index.ts` 中用於控制台輸出。
  - AC2：執行 `npm run dev` 或 `npm start` 通過日誌記錄啟動消息。
  - AC3：運行應用程序創建基礎輸出目錄（例如 `./output` 在 `.env` 中定義）如果它尚不存在。
  - AC4：運行應用程序在基礎輸出目錄中創建日期標記子目錄（例如 `./output/2025-05-04`）如果它尚不存在。
  - AC5：應用程序記錄一條消息，指示當前運行所使用的日期標記輸出目錄的完整路徑。
  - AC6：應用程序在執行這些設置步驟後正常退出（目前）。

## 變更紀錄

| 變更 | 日期       | 版本 | 說明        | 作者 |
| ---- | ---------- | ---- | ----------- | ---- |
| 初稿 | 2025-05-04 | 0.1  | Epic 1 初稿 | 2-pm |

# Epic 2 檔案

# Epic 2：HN 資料擷取與保存

**目標：** 從 Algolia HN API 擷取前 10 名故事及其留言（遵守數量限制），並將這些原始資料保存到 Epic 1 所建立的日期標記輸出目錄。實作階段測試工具以驗證擷取功能。

## 使用者故事清單

### Story 2.1: 實作 Algolia HN API 客戶端

- **使用者故事 / 目標：** 作為開發者，我希望有一個專用的客戶端模組來與 Algolia Hacker News Search API 互動，以便擷取故事和留言的邏輯能被封裝、重複利用，並且使用必要的原生 `Workspace` API。
- **詳細需求：**
  - 建立新模組：`src/clients/algoliaHNClient.ts`。
  - 在客戶端中實作非同步函式 `WorkspaceTopStories`：
    - 使用原生 `Workspace` 呼叫 Algolia HN Search API 前頁故事的端點（例如 `http://hn.algolia.com/api/v1/search?tags=front_page&hitsPerPage=10`）。如有需要可調整 `hitsPerPage` 以確保取得 10 則故事。
    - 解析 JSON 回應。
    - 擷取每則故事所需的中繼資料：`objectID`（作為 `storyId`）、`title`、`url`（文章網址）、`points`、`num_comments`。若 `url` 欄位缺漏需妥善處理（記錄警告，若後續需要網址可考慮略過該故事）。
    - 為每則故事組合出 `hnUrl`（例如 `https://news.ycombinator.com/item?id={storyId}`）。
    - 回傳結構化的故事物件陣列。
  - 在客戶端中另外實作非同步函式 `WorkspaceCommentsForStory`：
    - 接收 `storyId` 與 `maxComments` 上限作為參數。
    - 使用原生 `Workspace` 呼叫特定故事留言的 Algolia HN Search API 端點（例如 `http://hn.algolia.com/api/v1/search?tags=comment,story_{storyId}&hitsPerPage={maxComments}`）。
    - 解析 JSON 回應。
    - 擷取所需留言資料：`objectID`（作為 `commentId`）、`comment_text`、`author`、`created_at`。
    - 篩掉 `comment_text` 為 null 或空字串的留言。確保最多只回傳 `maxComments` 筆。
    - 回傳結構化的留言物件陣列。
  - 在 `Workspace` 呼叫外層使用 `try...catch` 實作基本錯誤處理，並檢查 `response.ok` 狀態。錯誤請使用 Epic 1 的 logger 工具記錄。
  - 定義 TypeScript 介面/型別，描述 API 回傳（故事、留言）的結構，以及客戶端函式回傳的資料型別（如 `Story`、`Comment`）。
- **驗收標準（ACs）：**
  - AC1：模組 `src/clients/algoliaHNClient.ts` 存在並且有匯出 `WorkspaceTopStories` 與 `WorkspaceCommentsForStory` 兩個函式。
  - AC2：呼叫 `WorkspaceTopStories` 會對正確的 Algolia 端點發出網路請求，並回傳一個 Promise，解析後取得包含指定中繼資料的 10 筆 `Story` 物件陣列。
  - AC3：呼叫 `WorkspaceCommentsForStory` 並傳入有效的 `storyId` 與 `maxComments` 上限時，會對正確的 Algolia 端點發出網路請求，並回傳一個 Promise，解析後取得 `Comment` 物件陣列（最多 `maxComments` 筆，且已過濾空留言）。
  - AC4：兩個函式皆於內部使用原生 `Workspace` API。
  - AC5：網路錯誤或 API 非成功回應（如狀態 4xx、5xx）會被捕捉並透過 logger 記錄。
  - AC6：相關 TypeScript 型別（如 `Story`、`Comment` 等）已定義並於客戶端模組內使用。

---

### Story 2.2: 將 HN 資料擷取整合進主流程

- **使用者故事 / 目標：** 作為開發者，我希望將 HN 資料擷取的邏輯整合進主應用程式流程（`src/index.ts`），以便執行應用程式時能在完成 Epic 1 的設定後，自動取得前 10 名故事及其留言。
- **詳細需求：**
  - 修改 `src/index.ts`（或由其呼叫的主 async 函式）中的主執行流程。
  - 匯入 `algoliaHNClient` 的相關函式。
  - 匯入組態模組以取得 `MAX_COMMENTS_PER_STORY`。
  - 在 Epic 1 的初始化（載入組態、logger 初始化、輸出目錄建立）完成後，呼叫 `WorkspaceTopStories()`。
  - 使用 logger 記錄取得的故事數量。
  - 逐一處理取得的 `Story` 物件陣列。
  - 對每個 `Story`，呼叫 `WorkspaceCommentsForStory()`，傳入 `story.storyId` 與設定的 `MAX_COMMENTS_PER_STORY`。
  - 將取得的留言存入該 `Story` 物件（例如新增 `comments: Comment[]` 屬性）。
  - 使用 logger 工具記錄進度（如「已取得 10 則故事」、「為故事 {storyId} 擷取最多 X 則留言...」）。
- **驗收標準（ACs）：**
  - AC1：執行 `npm run dev` 會依序執行 Epic 1 的初始化，再擷取故事，接著為每則故事擷取留言。
  - AC2：日誌明確顯示擷取故事的開始與完成，以及針對 10 則故事各自留言擷取的開始。
  - AC3：`MAX_COMMENTS_PER_STORY` 的設定值會自組態讀取並用於留言擷取函式呼叫。
  - AC4：執行結束後，記憶體中的故事物件會包含巢狀的留言物件陣列。（可用 debugger 或暫時性 log 驗證）

---

### Story 2.3: 將取得的 HN 資料本地保存

- **使用者故事 / 目標：** 作為開發者，我希望將取得的 HN 故事（含留言）存為 JSON 檔到日期標記的輸出目錄，讓原始資料能被後續流程或除錯使用。
- **詳細需求：**
  - 定義輸出檔案的 JSON 結構。例如：`{ storyId: "...", title: "...", url: "...", hnUrl: "...", points: ..., fetchedAt: "ISO_TIMESTAMP", comments: [{ commentId: "...", text: "...", author: "...", createdAt: "ISO_TIMESTAMP", ... }, ...] }`。需包含資料擷取的時間戳。
  - 匯入 Node.js 的 `fs`（特別是 `fs.writeFileSync`）與 `path` 模組。
  - 在主流程（`src/index.ts`）中，於處理每則故事（留言已擷取並存入物件後）：
    - 取得日期標記輸出目錄的完整路徑（Epic 1 已決定）。
    - 組合該故事資料檔名：`{storyId}_data.json`。
    - 使用 `path.join()` 組合完整檔案路徑。
    - 以 `JSON.stringify(storyObject, null, 2)` 將完整故事物件（含留言與擷取時間）序列化為易讀 JSON 字串。
    - 用 `fs.writeFileSync()` 寫入檔案。需包在 `try...catch` 區塊內進行錯誤處理。
  - 用 logger 記錄每則故事資料檔案保存成功或寫檔錯誤。
- **驗收標準（ACs）：**
  - AC1：執行 `npm run dev` 後，日期標記的輸出目錄（如 `./output/YYYY-MM-DD/`）會有 10 個檔案，檔名為 `{storyId}_data.json`。
  - AC2：每個 JSON 檔都包含一則故事的有效 JSON，內含中繼資料、擷取時間戳、留言陣列，結構符合定義。
  - AC3：每個檔案中的 `comments` 陣列數量不超過 `MAX_COMMENTS_PER_STORY`。
  - AC4：日誌會記錄每則故事資料檔案的保存嘗試與成功/失敗。

---

### Story 2.4: 實作 HN 擷取階段測試工具

- **使用者故事 / 目標：** 作為開發者，我希望有一個獨立可執行的腳本，僅執行 HN 資料的擷取與保存，方便我獨立測試這個階段而不需跑完整流程。
- **詳細需求：**
  - 建立新獨立腳本檔案：`src/stages/fetch_hn_data.ts`。
  - 此腳本需進行本階段所需的基本初始化：logger 初始化、載入組態（`.env`）、判斷與建立輸出目錄（可重用或複製 Epic 1/`src/index.ts` 的邏輯）。
  - 然後執行核心邏輯：呼叫 `algoliaHNClient.fetchTopStories` 取得故事，再依設定呼叫 `algoliaHNClient.fetchCommentsForStory` 取得留言，最後用 `fs.writeFileSync` 將結果存成 JSON 檔（複用 Story 2.3 的邏輯）。
  - 全程用 logger 記錄進度。
  - 在 `package.json` 的 `"scripts"` 區段新增指令：`"stage:fetch": "ts-node src/stages/fetch_hn_data.ts"`。
- **驗收標準（ACs）：**
  - AC1：檔案 `src/stages/fetch_hn_data.ts` 存在。
  - AC2：`stage:fetch` 指令已在 `package.json` 的 `scripts` 中定義。
  - AC3：執行 `npm run stage:fetch` 能順利完成，僅進行初始化、擷取、保存步驟。
  - AC4：執行 `npm run stage:fetch` 會在正確的日期標記輸出目錄產生 10 個 `{storyId}_data.json` 檔案，內容與主流程一致。
  - AC5：`npm run stage:fetch` 所產生日誌僅反映擷取與保存步驟，不包含後續流程。

## 變更紀錄

| 變更 | 日期       | 版本 | 說明        | 作者 |
| ---- | ---------- | ---- | ----------- | ---- |
| 初稿 | 2025-05-04 | 0.1  | Epic 2 初稿 | 2-pm |

# Epic 3 檔案

# Epic 3：文章爬取與保存

**目標：** 實作盡力而為的文章爬取機制，從取得的 HN 故事所附的外部網址抓取並擷取純文字內容。需能優雅處理失敗，並將成功爬取的文字本地保存。實作階段測試工具以驗證爬取功能。

## Story List

### Story 3.1: 實作基本文章爬取模組

- **使用者故事 / 目標：** 作為開發者，我希望有一個模組，嘗試從 URL 獲取 HTML 並使用基本方法提取主要文章文本，優雅地處理常見失敗，以便為摘要準備文章內容。
- **詳細需求：**
  - 創建一個新模組：`src/scraper/articleScraper.ts`。
  - 添加合適的 HTML 解析/提取庫依賴（例如，推薦使用 `@extractus/article-extractor` 以簡化，或 `cheerio` 以獲得更多控制）。執行 `npm install @extractus/article-extractor --save-prod`（或選擇的替代品）。
  - 在模組中實作一個非同步函式 `scrapeArticle(url: string): Promise<string | null>`。
  - 在函式內：
    - 使用原生 `Workspace` 從 `url` 獲取內容。設置合理的超時（例如 10-15 秒）。包括 `User-Agent` 標頭以模擬瀏覽器。
    - 使用 `try...catch` 處理潛在的 `Workspace` 錯誤（網路錯誤、超時）。
    - 檢查 `response.ok` 狀態。如果不正常，記錄錯誤並返回 `null`。
    - 檢查回應的 `Content-Type` 標頭。如果不指示 HTML（例如，不包含 `text/html`），記錄警告並返回 `null`。
    - 如果收到 HTML，嘗試使用選擇的庫提取主要文章文本（推薦使用 `article-extractor`）。
    - 將提取邏輯包裝在 `try...catch` 中以處理庫特定的錯誤。
    - 如果成功，返回提取的純文本字符串。確保它僅是文本，而不是 HTML 標記。
    - 如果提取失敗或結果為空內容，返回 `null`。
  - 記錄所有重要事件、錯誤或返回 null 的原因（例如 "Scraping URL..."、"Fetch failed:"、"Non-HTML content type:"、"Extraction failed:"、"Successfully extracted text"）使用日誌工具。
  - 根據需要定義 TypeScript 類型/介面。
- **驗收標準（ACs）：**
  - AC1：`articleScraper.ts` 模組存在並導出 `scrapeArticle` 函式。
  - AC2：選擇的爬取庫（例如 `@extractus/article-extractor`）已添加到 `package.json` 的 `dependencies` 中。
  - AC3：`scrapeArticle` 使用原生 `Workspace`，並設置超時和 User-Agent 標頭。
  - AC4：`scrapeArticle` 正確處理獲取錯誤、非 OK 回應和非 HTML 內容類型，通過日誌記錄並返回 `null`。
  - AC5：`scrapeArticle` 使用選擇的庫嘗試從有效的 HTML 內容中提取文本。
  - AC6：`scrapeArticle` 在成功時返回提取的純文本，並在任何失敗（獲取、非 HTML、提取錯誤、空結果）時返回 `null`。
  - AC7：在過程中產生相關日誌以顯示成功、失敗模式和遇到的錯誤。

---

### Story 3.2: 將文章爬取整合進主流程

- **使用者故事 / 目標：** 作為開發者，我希望將文章爬取器整合進主流程（`src/index.ts`），在獲取數據後，嘗試為每個 HN 故事爬取文章。
- **詳細需求：**
  - 修改 `src/index.ts` 中的主執行流程。
  - 從 `src/scraper/articleScraper.ts` 導入 `scrapeArticle` 函式。
  - 在主循環中遍歷獲取的故事（在 Epic 2 中獲取評論後）：
    - 檢查 `story.url` 是否存在並似乎是有效的 HTTP/HTTPS URL。簡單檢查以 `http://` 或 `https://` 開頭即可。
    - 如果 URL 缺失或無效，記錄警告（"Skipping scraping for story {storyId}: Missing or invalid URL"）並繼續到下一個故事的處理步驟。
    - 如果存在有效的 URL，記錄（"Attempting to scrape article for story {storyId} from {story.url}"）。
    - 調用 `await scrapeArticle(story.url)`。
    - 將結果（提取的文本字符串或 `null`）存儲在內存中，與故事對象關聯（例如，新增屬性 `articleContent: string | null`）。
    - 清楚地記錄結果（例如，"Successfully scraped article for story {storyId}"、"Failed to scrape article for story {storyId}"）。
- **驗收標準（ACs）：**
  - AC1：執行 `npm run dev` 執行 Epic 1 和 2 步驟，然後嘗試為具有有效 URL 的故事進行文章爬取。
  - AC2：缺失或無效 URL 的故事被跳過，並生成相應的日誌消息。
  - AC3：對於具有有效 URL 的故事，調用 `scrapeArticle` 函式。
  - AC4：日誌清楚地指示每個相關故事的爬取嘗試的開始和成功/失敗結果。
  - AC5：在此階段後，內存中的故事對象包含 `articleContent` 屬性，持有爬取的文本（字符串）或 `null` 如果爬取被跳過或失敗。

---

### Story 3.3: 本地保存爬取的文章文本

- **使用者故事 / 目標：** 作為開發者，我希望將成功爬取的文章文本保存到每個故事的單獨本地檔案中，以便在摘要階段使用文本內容。
- **詳細需求：**
  - 如果尚未在 `src/index.ts` 中導入 Node.js `fs` 和 `path` 模組，則進行導入。
  - 在主流程（`src/index.ts`）中，立即在成功調用 `scrapeArticle` 之後（結果為非空字符串）：
    - 獲取當前日期標記輸出目錄的完整路徑。
    - 構建檔案名：`{storyId}_article.txt`。
    - 使用 `path.join()` 構建完整檔案路徑。
    - 獲取成功爬取的文章文本字符串（`articleContent`）。
    - 使用 `fs.writeFileSync(fullPath, articleContent, 'utf-8')` 將文本保存到檔案中。包裝在 `try...catch` 中以處理檔案系統錯誤。
    - 記錄檔案成功保存的消息（例如，"Saved scraped article text to {filename}"）或遇到的任何檔案寫入錯誤。
  - 確保如果 `scrapeArticle` 返回 `null`（由於跳過或失敗），則不會創建 `_article.txt` 檔案。
- **驗收標準（ACs）：**
  - AC1：在執行 `npm run dev` 後，日期標記的輸出目錄僅包含 `_article.txt` 檔案，這些檔案僅對於 `scrapeArticle` 成功且返回文本內容的故事存在。
  - AC2：每個文章文本檔案的名稱為 `{storyId}_article.txt`。
  - AC3：每個 `_article.txt` 檔案的內容為 `scrapeArticle` 返回的純文本字符串。
  - AC4：日誌確認每個 `_article.txt` 檔案的成功寫入或報告特定的檔案寫入錯誤。
  - AC5：不會創建空的 `_article.txt` 檔案。僅在爬取成功時存在檔案。

---

### Story 3.4: 實作爬取階段測試工具

- **使用者故事 / 目標：** 作為開發者，我希望有一個獨立可執行的腳本，僅執行文章爬取與保存，方便我獨立測試這個階段而不需跑完整流程。
- **詳細需求：**
  - 創建一個新的獨立腳本檔案：`src/stages/scrape_articles.ts`。
  - 導入必要的模組：`fs`、`path`、`logger`、`config`、`scrapeArticle`。
  - 此腳本應：
    - 初始化日誌。
    - 載入配置（以獲取 `OUTPUT_DIR_PATH`）。
    - 確定目標日期標記目錄路徑（例如 `${OUTPUT_DIR_PATH}/YYYY-MM-DD`，使用當前日期或可能的 CLI 參數）。確保此目錄存在。
    - 讀取目錄內容並識別所有 `{storyId}_data.json` 檔案。
    - 對於每個找到的 `_data.json` 檔案：
      - 讀取並解析 JSON 內容。
      - 擷取 `storyId` 和 `url`。
      - 如果存在有效的 `url`，調用 `await scrapeArticle(url)`。
      - 如果爬取成功（返回文本），將文本保存到相同目錄中的 `{storyId}_article.txt`（使用 Story 3.3 的邏輯）。如果檔案存在則覆蓋。
      - 記錄每個處理的故事的進度和結果（跳過/成功/失敗）。
  - 在 `package.json` 中新增腳本命令：`"stage:scrape": "ts-node src/stages/scrape_articles.ts"`。
- **驗收標準（ACs）：**
  - AC1：檔案 `src/stages/scrape_articles.ts` 存在。
  - AC2：腳本 `stage:scrape` 在 `package.json` 中定義。
  - AC3：執行 `npm run stage:scrape`（假設之前的 `stage:fetch` 執行後存在目錄）能讀取這些檔案。
  - AC4：腳本對於在 JSON 檔案中找到的有效 URL 調用 `scrapeArticle`。
  - AC5：腳本在目標目錄中創建/更新 `{storyId}_article.txt` 檔案，反映成功爬取的文章。
  - AC6：腳本記錄其行動（讀取檔案、嘗試爬取、保存結果）對於每個處理的故事 ID。
  - AC7：腳本僅基於本地 `_data.json` 檔案和從外部文章 URL 獲取，不調用 Algolia HN API。

## 變更紀錄

| 變更 | 日期       | 版本 | 說明        | 作者 |
| ---- | ---------- | ---- | ----------- | ---- |
| 初稿 | 2025-05-04 | 0.1  | Epic 3 初稿 | 2-pm |

# Epic 4 檔案

# Epic 4：LLM 摘要生成與保存

**目標：** 與已設定的本地 Ollama 實例整合，為成功爬取的文章內容與取得的留言生成摘要，並將這些摘要本地保存。實作階段測試工具以驗證摘要功能。

## Story List

### Story 4.1: 實作 Ollama 客戶端模組

- **使用者故事 / 目標：** 作為開發者，我希望有一個客戶端模組來通過 HTTP 與配置的 Ollama API 端點互動，處理文本生成的請求和回應，以便能夠以程式化方式生成摘要。
- **詳細需求：**
  - **前提條件：** 確保本地 Ollama 實例已安裝並運行，通過 `.env` 中定義的 URL（`OLLAMA_ENDPOINT_URL`）可訪問，並且 `.env` 中指定的模型（`OLLAMA_MODEL`）已下載（例如，通過 `ollama pull model_name`）。此設置的說明應在專案 README 中。
  - 創建一個新模組：`src/clients/ollamaClient.ts`。
  - 實作一個非同步函式 `generateSummary(promptTemplate: string, content: string): Promise<string | null>`。 _(注意：參數名稱已更改以提高清晰度)_
  - 在 `.env.example` 中新增配置變數 `OLLAMA_ENDPOINT_URL`（例如 `http://localhost:11434`）和 `OLLAMA_MODEL`（例如 `llama3`）。確保它們通過配置模組（`src/utils/config.ts`）載入。用實際值更新本地 `.env`。在 `.env.example` 中新增可選的 `OLLAMA_TIMEOUT_MS`，默認為 `120000`。
  - 在 `generateSummary` 中：
    - 使用 `promptTemplate` 和提供的 `content` 構建完整的提示字符串（例如，替換模板中的佔位符 `{Content Placeholder}`，或如果模板簡單則進行簡單串接）。
    - 構建 Ollama API 請求有效載荷（JSON）：`{ model: configured_model, prompt: full_prompt, stream: false }`。參考 Ollama `/api/generate` 文檔和 `docs/data-models.md`。
    - 使用原生 `Workspace` 向配置的 Ollama 端點 + `/api/generate` 發送 POST 請求。設置適當的標頭（`Content-Type: application/json`）。使用配置的 `OLLAMA_TIMEOUT_MS` 或合理的默認值（例如 2 分鐘）。
    - 使用 `try...catch` 處理 `Workspace` 錯誤（網路、超時）。
    - 檢查 `response.ok`。如果不正常，記錄狀態/錯誤並返回 `null`。
    - 解析來自 Ollama 的 JSON 回應。提取生成的文本（通常在 `response` 欄位中）。參考 `docs/data-models.md`。
    - 檢查 Ollama 回應結構本身的潛在錯誤（例如，`error` 欄位）。
    - 在成功時返回提取的摘要字符串。在任何失敗時返回 `null`。
    - 記錄關鍵事件：發起請求（提及模型）、接收回應、成功、失敗原因，可能的請求/回應時間使用日誌。
  - 在 `src/types/ollama.ts` 中定義 Ollama 請求有效載荷和預期回應結構所需的 TypeScript 類型（參考 `docs/data-models.md`）。
- **驗收標準（ACs）：**
  - AC1：`ollamaClient.ts` 模組存在並導出 `generateSummary`。
  - AC2：`OLLAMA_ENDPOINT_URL` 和 `OLLAMA_MODEL` 在 `.env.example` 中定義，通過配置載入並被客戶端使用。可選的 `OLLAMA_TIMEOUT_MS` 被處理。
  - AC3：`generateSummary` 向配置的 Ollama 端點/路徑發送格式正確的 POST 請求（模型、基於模板和內容的完整提示、stream:false），使用原生 `Workspace`。
  - AC4：網路錯誤、超時和非 OK API 回應被優雅地處理，記錄並導致返回 `null`（前提是 Ollama 服務正在運行）。
  - AC5：成功的 Ollama 回應被正確解析，生成的文本被提取並作為字符串返回。
  - AC6：意外的 Ollama 回應格式或內部錯誤（例如 `{"error": "..."}`）被處理、記錄並導致返回 `null`。
  - AC7：日誌提供客戶端與 Ollama API 互動的可見性。

---

### Story 4.2: 定義摘要提示

- **使用者故事 / 目標：** 作為開發者，我希望為生成文章摘要和 HN 討論摘要定義標準化的基本提示，並集中記錄，確保一致的指令發送給 LLM。
- **詳細需求：**
  - 定義兩個標準化的基本提示（`ARTICLE_SUMMARY_PROMPT`、`DISCUSSION_SUMMARY_PROMPT`）**並在 `docs/prompts.md` 中記錄**。
  - 確保這些提示在應用程式代碼中可訪問，例如，通過在專用模組中定義它們作為導出常量，如 `src/utils/prompts.ts`，該模組從 `docs/prompts.md` 中讀取或鏡像內容。
- **驗收標準（ACs）：**
  - AC1：`ARTICLE_SUMMARY_PROMPT` 文本在 `docs/prompts.md` 中定義，並包含適當的指導內容。
  - AC2：`DISCUSSION_SUMMARY_PROMPT` 文本在 `docs/prompts.md` 中定義，並包含適當的指導內容。
  - AC3：在應用程式代碼中（例如，通過 `src/utils/prompts.ts`）可用的提示文本在 `docs/prompts.md` 中記錄，供 Ollama 客戶端整合使用。

---

### Story 4.3: 將摘要整合進主流程

- **使用者故事 / 目標：** 作為開發者，我希望將 Ollama 客戶端整合進主流程，為每個故事的爬取文章文本（如果可用）和獲取的評論生成摘要，使用集中定義的提示並處理潛在的評論長度限制。
- **詳細需求：**
  - 修改 `src/index.ts` 或 `src/core/pipeline.ts` 中的主執行流程。
  - 導入 `ollamaClient.generateSummary` 和提示常量/變數（例如，從 `src/utils/prompts.ts`，該模組反映 `docs/prompts.md`）。
  - 從 `.env` 通過配置工具載入可選的 `MAX_COMMENT_CHARS_FOR_SUMMARY` 配置值。
  - 在主循環中遍歷故事（在 Epic 3 中爬取/持久化文章後）：
  - **文章摘要生成：**
    - 檢查 `story` 對象是否具有非空的 `articleContent`。
    - 如果是：記錄 "Attempting article summarization for story {storyId}"，調用 `await generateSummary(ARTICLE_SUMMARY_PROMPT, story.articleContent)`，將結果（字符串或 null）存儲為 `story.articleSummary`，記錄成功/失敗。
    - 如果否：設置 `story.articleSummary = null`，記錄 "Skipping article summarization: No content"。
  - **討論摘要生成：**
    - 檢查 `story` 對象是否具有非空的 `comments` 陣列。
    - 如果是：
      - 將 `story.comments` 陣列格式化為適合 LLM 提示的單一文本塊（例如，將 `comment.text` 串接並使用分隔符如 `---`）。
      - **檢查截斷限制：** 如果 `MAX_COMMENT_CHARS_FOR_SUMMARY` 配置為正數且 `formattedCommentsText` 長度超過該限制，則將 `formattedCommentsText` 截斷至該限制並記錄警告："Comment text truncated to {limit} characters for summarization for story {storyId}"。
      - 記錄 "Attempting discussion summarization for story {storyId}"。
      - 調用 `await generateSummary(DISCUSSION_SUMMARY_PROMPT, formattedCommentsText)`。 _(傳遞可能被截斷的文本)_
      - 將結果（字符串或 null）存儲為 `story.discussionSummary`。記錄成功/失敗。
    - 如果否：設置 `story.discussionSummary = null`，記錄 "Skipping discussion summarization: No comments"。
- **驗收標準（ACs）：**
  - AC1：執行 `npm run dev` 執行 Epic 1-3 步驟，然後嘗試使用 Ollama 客戶端進行摘要生成。
  - AC2：僅在故事存在 `articleContent` 時嘗試文章摘要。
  - AC3：僅在故事存在 `comments` 時嘗試討論摘要。
  - AC4：`generateSummary` 使用正確的提示（與 `docs/prompts.md` 一致）和相應內容（文章文本或格式化/可能被截斷的評論）進行調用。
  - AC5：如果 `MAX_COMMENT_CHARS_FOR_SUMMARY` 設置且評論文本超過該限制，傳遞給 `generateSummary` 的文本被截斷，並記錄警告。
  - AC6：日誌清楚地指示每個故事的文章和討論摘要生成嘗試的開始、成功或失敗（包括來自客戶端的 null 返回）。
  - AC7：內存中的故事對象現在包含 `articleSummary`（字符串/null）和 `discussionSummary`（字符串/null）屬性。

---

### Story 4.4: 本地保存生成的摘要

- **使用者故事 / 目標：** 作為開發者，我希望將生成的文章和討論摘要（或 null 佔位符）保存到每個故事的本地 JSON 檔案中，使其可用於電子郵件組裝階段。
- **詳細需求：**
  - 定義摘要輸出檔案的結構：`{storyId}_summary.json`。內容範例：`{ "storyId": "...", "articleSummary": "...", "discussionSummary": "...", "summarizedAt": "ISO_TIMESTAMP" }`。注意 `articleSummary` 和 `discussionSummary` 可以為 null。
  - 在 `src/index.ts` 或 `src/core/pipeline.ts` 中導入 `fs` 和 `path`（如有需要）。
  - 在主流程循環中，在完成每個故事的 _兩個_ 摘要嘗試（文章和討論）後：
    - 創建一個摘要結果對象，包含 `storyId`、`articleSummary`（字符串或 null）、`discussionSummary`（字符串或 null）和當前 ISO 時間戳（`new Date().toISOString()`）。同時將此時間戳添加到內存中的 `story` 對象（`story.summarizedAt`）。
    - 獲取日期標記輸出目錄的完整路徑。
    - 構建檔案名：`{storyId}_summary.json`。
    - 使用 `path.join()` 構建完整檔案路徑。
    - 將摘要結果對象序列化為 JSON（`JSON.stringify(..., null, 2)`）。
    - 使用 `fs.writeFileSync` 將 JSON 保存到檔案中，包裝在 `try...catch` 中。
    - 記錄摘要檔案的成功保存或任何檔案寫入錯誤。
- **驗收標準（ACs）：**
  - AC1：在執行 `npm run dev` 後，日期標記的輸出目錄包含 10 個檔案，檔名為 `{storyId}_summary.json`。
  - AC2：每個 `_summary.json` 檔案包含有效的 JSON，符合定義的結構。
  - AC3：`articleSummary` 欄位包含生成的摘要字符串（如果成功），否則為 null。
  - AC4：`discussionSummary` 欄位包含生成的摘要字符串（如果成功），否則為 null。
  - AC5：`summarizedAt` 欄位中存在有效的 ISO 時間戳。
  - AC6：日誌確認每個摘要檔案的成功寫入或報告檔案系統錯誤。

---

### Story 4.5: 實作摘要階段測試工具

- **使用者故事 / 目標：** 作為開發者，我希望有一個獨立的腳本/命令來測試 LLM 摘要邏輯，使用本地持久化的數據（HN 評論、爬取的文章文本），允許獨立測試提示和 Ollama 互動。
- **詳細需求：**
  - 創建一個新的獨立腳本檔案：`src/stages/summarize_content.ts`。
  - 導入必要的模組：`fs`、`path`、`logger`、`config`、`ollamaClient`、提示常量（例如，來自 `src/utils/prompts.ts`）。
  - 此腳本應：
    - 初始化日誌，載入配置（Ollama 端點/模型、輸出目錄、**可選的 `MAX_COMMENT_CHARS_FOR_SUMMARY`**）。
    - 確定目標日期標記目錄路徑。
    - 找到目錄中的所有 `{storyId}_data.json` 檔案。
    - 對於每個找到的 `storyId`：
      - 讀取 `{storyId}_data.json` 以獲取評論。將其格式化為單一文本塊。
      - _嘗試_ 讀取 `{storyId}_article.txt`。優雅地處理檔案未找到的情況。存儲內容或 null。
      - 調用 `ollamaClient.generateSummary` 以文章文本（如果不為 null）使用 `ARTICLE_SUMMARY_PROMPT`。
      - **應用截斷邏輯：** 如果存在評論，檢查 `MAX_COMMENT_CHARS_FOR_SUMMARY` 並在需要時截斷格式化的評論文本塊，記錄警告。
      - 調用 `ollamaClient.generateSummary` 以格式化的評論（如果存在評論）使用 `DISCUSSION_SUMMARY_PROMPT` _(傳遞可能被截斷的文本)_。
      - 構建摘要結果對象（包含摘要或 null 和時間戳）。
      - 將結果對象保存到 `{storyId}_summary.json` 中（使用 Story 4.4 的邏輯），如果存在則覆蓋。
      - 記錄進度（讀取檔案、調用 Ollama、截斷警告、保存結果）對於每個處理的故事 ID。
  - 在 `package.json` 中新增腳本：`"stage:summarize": "ts-node src/stages/summarize_content.ts"`。
- **驗收標準（ACs）：**
  - AC1：檔案 `src/stages/summarize_content.ts` 存在。
  - AC2：腳本 `stage:summarize` 在 `package.json` 中定義。
  - AC3：執行 `npm run stage:summarize`（在 `stage:fetch` 和 `stage:scrape` 執行後）讀取 `_data.json` 並嘗試從目標目錄讀取 `_article.txt` 檔案。
  - AC4：腳本使用正確的提示（與 `docs/prompts.md` 一致）和僅從本地檔案派生的內容調用 `ollamaClient`（需要 Ollama 服務運行，根據 Story 4.1 的前提條件）。
  - AC5：如果設置了 `MAX_COMMENT_CHARS_FOR_SUMMARY` 並適用，則在調用客戶端之前截斷評論文本，並記錄警告。
  - AC6：腳本在目標目錄中創建/更新 `{storyId}_summary.json` 檔案，反映 Ollama 調用的結果（摘要或 null）。
  - AC7：日誌顯示腳本處理每個在本地找到的故事 ID，與 Ollama 互動並保存結果。
  - AC8：腳本僅基於本地 `_data.json` 檔案和從外部文章 URL 獲取，不調用 Algolia API。

## 變更紀錄

| 變更                     | 日期       | 版本 | 說明                           | 作者        |
| ------------------------ | ---------- | ---- | ------------------------------ | ----------- |
| 整合 prompts.md 參考     | 2025-05-04 | 0.3  | 更新故事 4.2、4.3、4.5         | 3-Architect |
| 新增 Ollama 前提條件說明 | 2025-05-04 | 0.2  | 新增有關本地 Ollama 設置的說明 | 2-pm        |
| 初稿                     | 2025-05-04 | 0.1  | Epic 4 的初稿                  | 2-pm        |

# Epic 5 檔案

# Epic 5：摘要彙整與電子郵件發送

**目標：** 彙整本地檔案中收集的故事資料與摘要，將其格式化為可閱讀的 HTML 電子郵件摘要，並使用 Nodemailer 與設定的憑證發送郵件。實作階段測試工具，支援 dry-run 選項以避免測試時寄出真實郵件。

## Story List

### Story 5.1: 實作電子郵件內容組裝器

- **使用者故事 / 目標：** 作為開發者，我希望有一個模組，從指定目錄中讀取持久化的故事元數據（`_data.json`）和摘要（`_summary.json`），整合渲染電子郵件摘要所需的必要信息。
- **詳細需求：**
  - 創建一個新模組：`src/email/contentAssembler.ts`。
  - 定義一個 TypeScript 類型/介面 `DigestData`，表示每個故事在電子郵件模板中所需的數據：`{ storyId: string, title: string, hnUrl: string, articleUrl: string | null, articleSummary: string | null, discussionSummary: string | null }`。
  - 實作一個非同步函式 `assembleDigestData(dateDirPath: string): Promise<DigestData[]>`。
  - 該函式應：
    - 使用 Node.js `fs` 讀取 `dateDirPath` 的內容。
    - 識別所有符合模式 `{storyId}_data.json` 的檔案。
    - 對於每個找到的 `storyId`：
      - 讀取並解析 `{storyId}_data.json` 檔案。擷取 `title`、`hnUrl` 和 `url`（用作 `articleUrl`）。優雅地處理潛在的檔案讀取/解析錯誤（記錄並跳過故事）。
      - 嘗試讀取並解析對應的 `{storyId}_summary.json` 檔案。優雅地處理檔案未找到或解析錯誤（將 `articleSummary` 和 `discussionSummary` 視為 null）。
      - 為故事構建 `DigestData` 對象，包括提取的元數據和摘要（或 null）。
    - 將所有成功構建的 `DigestData` 對象收集到一個數組中。
    - 返回該數組。如果所有先前階段成功，則理想上應包含 10 項。
  - 記錄進度（例如，"Assembling digest data from directory..."、"Processing story {storyId}..."）以及在檔案處理過程中遇到的任何錯誤，使用日誌。
- **驗收標準（ACs）：**
  - AC1：`contentAssembler.ts` 模組存在並導出 `assembleDigestData` 和 `DigestData` 類型。
  - AC2：`assembleDigestData` 正確從提供的目錄路徑讀取 `_data.json` 檔案。
  - AC3：它嘗試讀取對應的 `_summary.json` 檔案，正確處理摘要檔案可能缺失或無法解析的情況（導致該故事的摘要為 null）。
  - AC4：該函式返回一個 Promise，解析為一個 `DigestData` 對象的數組，從檔案中提取的數據填充。
  - AC5：在檔案讀取或 JSON 解析過程中出現的錯誤被記錄，該函式返回成功處理的故事數據。

---

### Story 5.2: 創建 HTML 電子郵件模板與渲染器

- **使用者故事 / 目標：** 作為開發者，我希望有一個基本的 HTML 電子郵件模板和一個函式來使用組裝的摘要數據渲染它，生成電子郵件主體的最終 HTML 內容。
- **詳細需求：**
  - 定義 HTML 結構。這可以通過函式中的模板字面量來完成，或可能使用簡單的模板檔案（例如，`src/email/templates/digestTemplate.html`）和 `fs.readFileSync`。模板字面量對於 MVP 更簡單。
  - 創建一個函式 `renderDigestHtml(data: DigestData[], digestDate: string): string`（例如，在 `src/email/contentAssembler.ts` 或新的 `templater.ts` 中）。
  - 該函式應生成一個 HTML 字符串，包含：
    - 主體中的適當標題（例如，`<h1>Hacker News Top 10 Summaries for ${digestDate}</h1>`）。
    - 遍歷 `data` 陣列的循環。
    - 對於 `data` 中的每個 `story`：
      - 顯示 `<h2><a href="${story.articleUrl || story.hnUrl}">${story.title}</a></h2>`。
      - 顯示 `<p><a href="${story.hnUrl}">View HN Discussion</a></p>`。
      - 條件顯示 `<h3>Article Summary</h3><p>${story.articleSummary}</p>` _僅當_ `story.articleSummary` 不為 null/空。
      - 條件顯示 `<h3>Discussion Summary</h3><p>${story.discussionSummary}</p>` _僅當_ `story.discussionSummary` 不為 null/空。
      - 包含分隔符（例如，`<hr style="margin-top: 20px; margin-bottom: 20px;">`）。
  - 使用基本的內聯 CSS 進行最小樣式設置（邊距等），以確保可讀性。避免複雜的佈局。
  - 返回完整的 HTML 文檔作為字符串。
- **驗收標準（ACs）：**
  - AC1：函式 `renderDigestHtml` 存在，接受摘要數據陣列和日期字符串。
  - AC2：該函式返回一個完整的 HTML 字符串。
  - AC3：生成的 HTML 包含標題和日期，並正確遍歷故事數據。
  - AC4：對於每個故事，HTML 顯示鏈接標題、HN 連結，並根據條件顯示文章和討論摘要及標題。
  - AC5：使用基本的分隔符和邊距以提高可讀性。HTML 簡單，能夠在大多數電子郵件客戶端中合理渲染。

---

### Story 5.3: 實作 Nodemailer 電子郵件發送器

- **使用者故事 / 目標：** 作為開發者，我希望有一個模組來使用 Nodemailer 發送生成的 HTML 電子郵件，並使用安全地存儲在環境檔案中的憑證進行配置。
- **詳細需求：**
  - 添加 Nodemailer 依賴：`npm install nodemailer @types/nodemailer --save-prod`。
  - 在 `.env.example`（和本地 `.env`）中添加所需的配置變數：`EMAIL_HOST`、`EMAIL_PORT`（例如，587）、`EMAIL_SECURE`（例如，`false` 用於 STARTTLS 在 587，`true` 用於 465）、`EMAIL_USER`、`EMAIL_PASS`、`EMAIL_FROM`（例如，`"Your Name <you@example.com>"`）、`EMAIL_RECIPIENTS`（以逗號分隔的列表）。
  - 創建一個新模組：`src/email/emailSender.ts`。
  - 實作一個非同步函式 `sendDigestEmail(subject: string, htmlContent: string): Promise<boolean>`。
  - 在函式內：
    - 從配置模組載入 `EMAIL_*` 變數。
    - 使用 `nodemailer.createTransport` 創建 Nodemailer 傳輸器，使用載入的配置（主機、端口、安全標誌、身份驗證：{用戶、密碼}）。
    - 使用 `transporter.verify()` 驗證傳輸器配置（可選但建議）。記錄驗證成功/失敗。
    - 將 `EMAIL_RECIPIENTS` 字符串解析為適合 `to` 欄位的數組或以逗號分隔的字符串。
    - 定義 `mailOptions`：`{ from: EMAIL_FROM, to: parsedRecipients, subject: subject, html: htmlContent }`。
    - 調用 `await transporter.sendMail(mailOptions)`。
    - 如果 `sendMail` 成功，記錄成功消息，包括結果中的 `messageId`。返回 `true`。
    - 如果 `sendMail` 失敗（拋出錯誤），使用日誌記錄錯誤。返回 `false`。
- **驗收標準（ACs）：**
  - AC1：已添加 `nodemailer` 和 `@types/nodemailer` 依賴。
  - AC2：在 `.env.example` 中定義 `EMAIL_*` 變數並從配置中載入。
  - AC3：`emailSender.ts` 模組存在並導出 `sendDigestEmail`。
  - AC4：`sendDigestEmail` 正確使用來自 `.env` 的配置創建 Nodemailer 傳輸器。嘗試驗證傳輸器（可選 AC）。
  - AC5：`to` 欄位根據 `EMAIL_RECIPIENTS` 正確填充。
  - AC6：`transporter.sendMail` 使用正確的 `from`、`to`、`subject` 和 `html` 選項進行調用。
  - AC7：電子郵件發送成功（包括消息 ID）或失敗被清楚記錄。
  - AC8：函式在成功發送時返回 `true`，否則返回 `false`。

---

### Story 5.4: 將電子郵件組裝和發送整合進主流程

- **使用者故事 / 目標：** 作為開發者，我希望主應用程式流程（`src/index.ts`）能協調最終步驟：組裝摘要數據、渲染 HTML，並在所有先前階段完成後觸發電子郵件發送。
- **詳細需求：**
  - 修改 `src/index.ts` 中的主執行流程。
  - 導入 `assembleDigestData`、`renderDigestHtml`、`sendDigestEmail`。
  - 在主循環（獲取、爬取、摘要和持久化故事後）完成後執行這些步驟：
    - 記錄 "Starting final digest assembly and email dispatch..."。
    - 確定當前日期標記輸出目錄的路徑。
    - 調用 `const digestData = await assembleDigestData(dateDirPath)`。
    - 檢查 `digestData` 陣列是否不為空。
      - 如果是：
        - 獲取當前日期字符串（例如，'YYYY-MM-DD'）。
        - `const htmlContent = renderDigestHtml(digestData, currentDate)`。
        - `const subject = \`BMad Hacker Daily Digest - ${currentDate}\``。
        - `const emailSent = await sendDigestEmail(subject, htmlContent)`。
        - 根據 `emailSent` 記錄最終結果（"Digest email sent successfully." 或 "Failed to send digest email."）。
      - 如果否（`digestData` 為空或組裝失敗）：
        - 記錄錯誤："Failed to assemble digest data or no data found. Skipping email."
    - 記錄 "BMad Hacker Daily Digest process finished."
- **驗收標準（ACs）：**
  - AC1：執行 `npm run dev` 執行所有階段（Epics 1-4），然後進行電子郵件組裝和發送。
  - AC2：`assembleDigestData` 在其他處理完成後正確調用，並使用輸出目錄路徑。
  - AC3：如果數據組裝成功，則 `renderDigestHtml` 和 `sendDigestEmail` 使用正確的數據、主題和 HTML 進行調用。
  - AC4：電子郵件發送步驟的最終成功或失敗被記錄。
  - AC5：如果 `assembleDigestData` 返回無數據，則跳過電子郵件發送，並記錄適當的消息。
  - AC6：應用程序記錄最終完成消息。

---

### Story 5.5: 實作電子郵件發送的階段測試工具

- **使用者故事 / 目標：** 作為開發者，我希望有一個獨立的腳本/命令來測試電子郵件組裝、渲染和發送邏輯，使用持久化的本地數據，包括關鍵的 `--dry-run` 選項，以防止在測試期間意外發送電子郵件。
- **詳細需求：**
  - 添加 `yargs` 依賴以進行參數解析：`npm install yargs @types/yargs --save-dev`。
  - 創建一個新的獨立腳本檔案：`src/stages/send_digest.ts`。
  - 導入必要的模組：`fs`、`path`、`logger`、`config`、`assembleDigestData`、`renderDigestHtml`、`sendDigestEmail`、`yargs`。
  - 使用 `yargs` 解析命令行參數，特別是查找 `--dry-run` 布林標誌（默認為 `false`）。允許可選參數以指定日期標記目錄，否則默認為當前日期。
  - 此腳本應：
    - 初始化日誌，載入配置。
    - 確定目標日期標記目錄路徑（從參數或默認）。記錄目標目錄。
    - 調用 `await assembleDigestData(dateDirPath)`。
    - 如果數據組裝成功且不為空：
      - 確定主題/標題的日期字符串。
      - 調用 `renderDigestHtml(digestData, dateString)` 獲取 HTML。
      - 構建主題字符串。
      - 檢查 `dryRun` 標誌：
        - 如果為 `true`：記錄 "DRY RUN enabled. Skipping actual email send."。記錄主題。將 `htmlContent` 保存到目標目錄中的檔案（例如，`_digest_preview.html`）。記錄預覽檔案已保存。
        - 如果為 `false`：記錄 "Live run: Attempting to send email..."。調用 `await sendDigestEmail(subject, htmlContent)`。根據返回值記錄成功/失敗。
    - 如果數據組裝失敗或為空，記錄錯誤。
  - 在 `package.json` 中新增腳本：`"stage:email": "ts-node src/stages/send_digest.ts --"`。`--` 允許傳遞參數，如 `--dry-run`。
- **驗收標準（ACs）：**
  - AC1：檔案 `src/stages/send_digest.ts` 存在。已添加 `yargs` 依賴。
  - AC2：腳本 `stage:email` 在 `package.json` 中定義，允許參數。
  - AC3：執行 `npm run stage:email -- --dry-run` 讀取本地數據，渲染 HTML，記錄意圖，將 `_digest_preview.html` 本地保存，並且不調用 `sendDigestEmail`。
  - AC4：執行 `npm run stage:email`（不帶 `--dry-run`）讀取本地數據，渲染 HTML，並且調用 `sendDigestEmail`，記錄結果。
  - AC5：腳本正確識別並根據 `--dry-run` 標誌採取行動。
  - AC6：日誌清楚區分乾跑和實跑，並報告成功/失敗。
  - AC7：腳本僅使用本地檔案和電子郵件配置/服務運行，不調用先前的管道階段（Algolia、爬取、Ollama）。

## 變更紀錄

| 變更 | 日期       | 版本 | 說明          | 作者 |
| ---- | ---------- | ---- | ------------- | ---- |
| 初稿 | 2025-05-04 | 0.1  | Epic 5 的初稿 | 2-pm |
