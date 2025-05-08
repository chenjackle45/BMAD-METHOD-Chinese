# Epic 3: 文章擷取與持久化

**目標：** 實作一個盡力而為的文章擷取機制，從與 HN 故事相關的外部 URL 中抓取並提取純文字內容。優雅地處理失敗情況，並將成功擷取的文字內容本地持久化。實作一個階段測試工具來測試擷取功能。

## 故事列表

### 故事 3.1: 實作基本文章擷取模組

- **使用者故事／目標：** 作為一名開發者，我希望有一個模組能嘗試從 URL 抓取 HTML 並使用基本方法提取主要文章文字，優雅地處理常見失敗情況，以便為摘要準備文章內容。
- **詳細需求：**
  - 建立一個新模組：`src/scraper/articleScraper.ts`。
  - 新增適合的 HTML 解析／提取函式庫相依套件（例如，推薦使用 `@extractus/article-extractor` 以簡化操作，或使用 `cheerio` 以獲得更多控制）。執行 `npm install @extractus/article-extractor --save-prod`（或選擇的替代方案）。
  - 在模組中實作一個非同步函式 `scrapeArticle(url: string): Promise<string | null>`。
  - 在函式內：
    - 使用原生 `Workspace` 從 `url` 抓取內容。設定合理的超時時間（例如 10-15 秒）。包含一個 `User-Agent` 標頭以模仿瀏覽器。
    - 使用 `try...catch` 處理可能的 `Workspace` 錯誤（網路錯誤、超時）。
    - 檢查 `response.ok` 狀態。如果不正常，記錄錯誤並返回 `null`。
    - 檢查回應的 `Content-Type` 標頭。如果不指向 HTML（例如，不包含 `text/html`），記錄警告並返回 `null`。
    - 如果收到 HTML，嘗試使用選定的函式庫（推薦 `article-extractor`）提取主要文章文字。
    - 將提取邏輯包裹在 `try...catch` 中以處理函式庫特定錯誤。
    - 如果成功，返回提取的純文字字串。確保僅為文字，無 HTML 標記。
    - 如果提取失敗或結果為空內容，返回 `null`。
  - 使用記錄器工具記錄所有重要事件、錯誤或返回 null 的原因（例如，「正在擷取 URL...」、「抓取失敗：」、「非 HTML 內容類型：」、「提取失敗：」、「成功提取文字」）。
  - 根據需要定義 TypeScript 類型／介面。
- **驗收標準（ACs）：**
  - AC1：`articleScraper.ts` 模組存在並匯出 `scrapeArticle` 函式。
  - AC2：選定的擷取函式庫（例如，`@extractus/article-extractor`）已新增至 `package.json` 的 `dependencies`。
  - AC3：`scrapeArticle` 使用原生 `Workspace`，並設有超時與 User-Agent 標頭。
  - AC4：`scrapeArticle` 正確處理抓取錯誤、非 OK 回應及非 HTML 內容類型，並記錄日誌與返回 `null`。
  - AC5：`scrapeArticle` 使用選定的函式庫嘗試從有效 HTML 內容中提取文字。
  - AC6：`scrapeArticle` 成功時返回提取的純文字，失敗時返回 `null`（抓取失敗、非 HTML、提取錯誤、空結果）。
  - AC7：相關日誌記錄成功、失敗模式及過程中遇到的錯誤。

---

### 故事 3.2: 將文章擷取整合至主工作流程

- **使用者故事／目標：** 作為一名開發者，我希望將文章擷取整合至主工作流程（`src/index.ts`），在抓取 HN 故事數據後，嘗試為每個具有有效 URL 的故事擷取文章。
- **詳細需求：**
  - 修改主執行流程於 `src/index.ts`。
  - 匯入 `src/scraper/articleScraper.ts` 中的 `scrapeArticle` 函式。
  - 在迭代抓取的故事的主迴圈中（在 Epic 2 中抓取評論後）：
    - 檢查 `story.url` 是否存在且為有效的 HTTP/HTTPS URL。簡單檢查是否以 `http://` 或 `https://` 開頭即可。
    - 如果 URL 缺失或無效，記錄警告（例如，「跳過故事 {storyId} 的擷取：缺失或無效的 URL」），並繼續處理下一個故事。
    - 如果存在有效 URL，記錄（例如，「嘗試為故事 {storyId} 擷取文章，來源 {story.url}」）。
    - 呼叫 `await scrapeArticle(story.url)`。
    - 將結果（提取的文字字串或 `null`）存儲於記憶體中，與故事物件關聯（例如，新增屬性 `articleContent: string | null`）。
    - 清楚記錄結果（例如，「成功擷取故事 {storyId} 的文章」、「擷取故事 {storyId} 的文章失敗」）。
- **驗收標準（ACs）：**
  - AC1：執行 `npm run dev` 執行 Epic 1 和 Epic 2 步驟，然後嘗試為具有有效 URL 的故事擷取文章。
  - AC2：缺失或無效 URL 的故事被跳過，並生成相應的日誌訊息。
  - AC3：對於具有有效 URL 的故事，呼叫 `scrapeArticle` 函式。
  - AC4：日誌清楚顯示每個相關故事的擷取嘗試開始及成功／失敗結果。
  - AC5：此階段後記憶體中的故事物件包含一個 `articleContent` 屬性，保存擷取的文字（字串）或 `null`（若擷取被跳過或失敗）。

---

### 故事 3.3: 本地持久化擷取的文章文字

- **使用者故事／目標：** 作為一名開發者，我希望將成功擷取的文章文字保存為每個故事的單獨本地檔案，以便在摘要階段使用文字內容作為輸入。
- **詳細需求：**
  - 如果 `src/index.ts` 中尚未匯入 Node.js 的 `fs` 和 `path` 模組，請匯入。
  - 在主工作流程（`src/index.ts`）中，於成功呼叫 `scrapeArticle` 並返回非空字串的故事後：
    - 獲取當前日期標記的輸出目錄的完整路徑。
    - 構造檔案名稱：`{storyId}_article.txt`。
    - 使用 `path.join()` 構造完整檔案路徑。
    - 獲取成功擷取的文章文字字串（`articleContent`）。
    - 使用 `fs.writeFileSync(fullPath, articleContent, 'utf-8')` 將文字保存至檔案。用 `try...catch` 包裹以處理檔案系統錯誤。
    - 記錄成功保存檔案的訊息（例如，「已保存擷取的文章文字至 {filename}」）或遇到的檔案寫入錯誤。
  - 確保若 `scrapeArticle` 返回 `null`（因跳過或失敗），則不建立 `_article.txt` 檔案。
- **驗收標準（ACs）：**
  - AC1：執行 `npm run dev` 後，日期標記的輸出目錄僅包含 `_article.txt` 檔案，且僅限於 `scrapeArticle` 成功並返回文字內容的故事。
  - AC2：每個文章文字檔案的名稱為 `{storyId}_article.txt`。
  - AC3：每個 `_article.txt` 檔案的內容為 `scrapeArticle` 返回的純文字字串。
  - AC4：日誌確認每個 `_article.txt` 檔案的成功寫入，或報告具體的檔案寫入錯誤。
  - AC5：不建立空的 `_article.txt` 檔案。僅在擷取成功時建立檔案。

---

### 故事 3.4: 實作擷取階段測試工具

- **使用者故事／目標：** 作為一名開發者，我希望有一個獨立的腳本／命令來測試文章擷取邏輯，使用本地檔案中的 HN 故事數據，允許獨立測試與除錯擷取功能。
- **詳細需求：**
  - 建立一個新的獨立腳本檔案：`src/stages/scrape_articles.ts`。
  - 匯入必要模組：`fs`、`path`、`logger`、`config`、`scrapeArticle`。
  - 腳本應：
    - 初始化記錄器。
    - 加載配置（以獲取 `OUTPUT_DIR_PATH`）。
    - 確定目標日期標記的目錄路徑（例如，`${OUTPUT_DIR_PATH}/YYYY-MM-DD`，使用當前日期或可能的可選 CLI 參數）。確保該目錄存在。
    - 讀取目錄內容並識別所有 `{storyId}_data.json` 檔案。
    - 對於找到的每個 `_data.json` 檔案：
      - 讀取並解析 JSON 內容。
      - 提取 `storyId` 和 `url`。
      - 如果存在有效的 `url`，呼叫 `await scrapeArticle(url)`。
      - 如果擷取成功（返回文字），將文字保存至 `{storyId}_article.txt`，位於相同目錄（使用故事 3.3 的邏輯）。若檔案已存在則覆蓋。
      - 記錄每個故事處理的進度與結果（跳過／成功／失敗）。
  - 在 `package.json` 中新增一個腳本命令：`"stage:scrape": "ts-node src/stages/scrape_articles.ts"`。若需要，稍後可新增參數解析以指定日期／目錄。
- **驗收標準（ACs）：**
  - AC1：檔案 `src/stages/scrape_articles.ts` 存在。
  - AC2：腳本 `stage:scrape` 已定義於 `package.json`。
  - AC3：執行 `npm run stage:scrape`（假設存在來自先前 `stage:fetch` 執行的 `_data.json` 檔案目錄）會讀取這些檔案。
  - AC4：腳本為 JSON 檔案中具有有效 URL 的故事呼叫 `scrapeArticle`。
  - AC5：腳本在目標目錄中建立／更新 `{storyId}_article.txt` 檔案，對應於成功擷取的文章。
  - AC6：腳本記錄其行為（讀取檔案、嘗試擷取、保存結果）以處理的每個故事 ID。
  - AC7：腳本僅基於本地 `_data.json` 檔案運行，並從外部文章 URL 抓取；不呼叫 Algolia HN API。

## 變更記錄

| 變更 | 日期       | 版本 | 描述          | 作者 |
| ---- | ---------- | ---- | ------------- | ---- |
| 初稿 | 2025-05-04 | 0.1  | Epic 3 的初稿 | 2-pm |
