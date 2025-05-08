好的，保持在 **SM 模式**。

正在為 **Epic 3: 文章擷取與持久化** 生成故事。這假設 Epic 1（專案設定、配置、日誌記錄、輸出目錄）和 Epic 2（HN 客戶端實現、資料擷取整合以及 `{storyId}_data.json` 檔案的本地持久化）已完成。

---

## **故事報告：Epic 3**

**檔案: ai/stories/3.1.story.md**

```markdown
# 故事 3.1: 實現基本文章擷取模組

**狀態:** 草稿

## 目標與背景

**使用者故事:** 作為一名開發者，我希望有一個模組能嘗試從 URL 擷取 HTML 並使用基本方法提取主要文章文字，能夠優雅地處理常見的失敗情況，從而為摘要準備文章內容。[220]

**背景:** 此故事引入了文章擷取功能。它建立了一個專用模組（`src/scraper/articleScraper.ts`），負責從外部文章 URL（來自 Epic 2 的 `{storyId}_data.json` 檔案中找到）擷取內容並提取純文字。它強調使用原生 `Workspace` 和簡單的提取函式庫（建議使用 `@extractus/article-extractor` [222, 873]），並且至關重要的是，根據 PRD [723, 724, 741] 的要求，能夠穩健地處理失敗（超時、非 HTML 內容、提取錯誤）。此模組將由主管道（故事 3.2）和階段測試器（故事 3.4）使用。[47, 55, 60, 63, 65]

## 詳細需求

- 建立一個新模組：`src/scraper/articleScraper.ts`。[221]
- 添加 `@extractus/article-extractor` 相依套件：`npm install @extractus/article-extractor --save-prod`。[222, 223, 873]
- 在模組中實現一個非同步函式 `scrapeArticle(url: string): Promise<string | null>`。[223, 224]
- 在函式內：
  - 使用原生 `Workspace` [749] 從 `url` 擷取內容。[224] 設置合理的超時（例如，通過 `AbortSignal.timeout()` 設置 15 秒，必要時通過 `SCRAPE_TIMEOUT_MS` [615] 配置）。包括 `User-Agent` 標頭（例如，`"BMadHackerDigest/0.1"` 或通過 `SCRAPER_USER_AGENT` [629] 配置）。[225]
  - 使用 `try...catch` 處理潛在的 `Workspace` 錯誤（網路錯誤、超時）。使用日誌記錄器（來自故事 1.4）記錄錯誤並返回 `null`。[226]
  - 檢查 `response.ok` 狀態。如果不正常，記錄錯誤（包括狀態碼）並返回 `null`。[226, 227]
  - 檢查回應的 `Content-Type` 標頭。如果未指示 HTML（例如，不包括 `text/html`），記錄警告並返回 `null`。[227, 228]
  - 如果收到 HTML（`response.text()`），嘗試使用 `@extractus/article-extractor` 提取主要文章文字。[229]
  - 將提取邏輯（`await articleExtractor.extract(htmlContent)`）包裹在 `try...catch` 中以處理函式庫特定錯誤。記錄錯誤並在失敗時返回 `null`。[230]
  - 如果成功且非空，返回提取的純文字（`article.content`）。確保它僅是文字，而不是 HTML 標記。[231]
  - 如果提取失敗或結果為空內容，返回 `null`。[232]
  - 使用日誌工具記錄所有重要事件、錯誤或返回 null 的原因（例如，"正在擷取 URL..."、"擷取失敗:"、"非正常狀態:"、"非 HTML 內容類型:"、"提取失敗:"、"成功提取 {url} 的文字"）。[233]
- 根據需要定義 TypeScript 類型/介面（儘管 `article-extractor` 類型可能已足夠）。[234]

## 驗收標準（ACs）

- AC1: `src/scraper/articleScraper.ts` 模組存在並導出 `scrapeArticle` 函式。[234]
- AC2: `@extractus/article-extractor` 函式庫已添加到 `package.json` 的 `dependencies` 中，並更新了 `package-lock.json`。[235]
- AC3: `scrapeArticle` 使用原生 `Workspace`，設置超時（默認或配置）和 User-Agent 標頭。[236]
- AC4: `scrapeArticle` 正確處理擷取錯誤（網路、超時）、非正常回應和非 HTML 內容類型，通過記錄具體原因並返回 `null`。[237]
- AC5: `scrapeArticle` 使用 `@extractus/article-extractor` 嘗試從通過 `response.text()` 擷取的有效 HTML 內容中提取文字。[238]
- AC6: `scrapeArticle` 在成功時返回提取的純文字字串，在任何失敗（擷取、非 HTML、提取錯誤、空結果）時返回 `null`。[239]
- AC7: 使用日誌工具記錄成功、不同失敗模式以及過程中遇到的錯誤。[240]

## 技術實現背景

**指導:** 使用以下細節進行實現。如有需要，請參考鏈接的 `docs/` 檔案以獲取更廣泛的背景。

- **相關檔案:**
  - 要建立的檔案：`src/scraper/articleScraper.ts`。
  - 要修改的檔案：`package.json`、`package-lock.json`。將可選的環境變數添加到 `.env.example`。
  - _(提示：請參閱 `docs/project-structure.md` [819] 以了解擷取器位置)。_
- **關鍵技術:**
  - TypeScript [846]、Node.js 22.x [851]、原生 `Workspace` API [863]。
  - `@extractus/article-extractor` 函式庫。[873]
  - 使用 `logger` 工具（故事 1.4）。
  - 使用 `config` 工具（故事 1.2），如果實現可配置的超時/User-Agent。
  - _(提示：請參閱 `docs/tech-stack.md` [839-905]）。_
- **API 互動/SDK 使用:**
  - 原生 `Workspace(url, { signal: AbortSignal.timeout(timeoutMs), headers: { 'User-Agent': userAgent } })`。[225]
  - 檢查 `response.ok`、`response.headers.get('Content-Type')`。[227, 228]
  - 獲取正文為文字：`await response.text()`。[229]
  - `@extractus/article-extractor`: `import articleExtractor from '@extractus/article-extractor'; const article = await articleExtractor.extract(htmlContent); return article?.content || null;` [229, 231]
- **資料結構:**
  - 函式簽名：`scrapeArticle(url: string): Promise<string | null>`。[224]
  - 使用提取器返回的 `article` 物件。
  - _(提示：請參閱 `docs/data-models.md` [498-547]）。_
- **環境變數:**
  - 可選：`SCRAPE_TIMEOUT_MS`（默認例如 15000）。[615]
  - 可選：`SCRAPER_USER_AGENT`（默認例如 "BMadHackerDigest/0.1"）。[629]
  - 如果使用，通過 `config.ts` 加載。
  - _(提示：請參閱 `docs/environment-vars.md` [548-638]）。_
- **程式碼標準說明:**
  - 使用 `async/await`。
  - 為 `Workspace` 和提取實現全面的 `try...catch` 塊。[226, 230]
  - 清楚地記錄錯誤和返回 `null` 的原因。[233]

## 任務/子任務

- [ ] 執行 `npm install @extractus/article-extractor --save-prod`。
- [ ] 建立 `src/scraper/articleScraper.ts`。
- [ ] 匯入日誌工具（可選配置）和 `articleExtractor`。
- [ ] 定義接受 `url` 的非同步函式 `scrapeArticle`。
- [ ] 為整個擷取/解析邏輯實現 `try...catch`。在 `catch` 中記錄錯誤並返回 `null`。
- [ ] 在 `try` 中：
  - [ ] 定義超時（默認或來自配置）。
  - [ ] 定義 User-Agent（默認或來自配置）。
  - [ ] 使用 URL、超時信號和 User-Agent 標頭呼叫原生 `Workspace`。
  - [ ] 檢查 `response.ok`。如果不正常，記錄狀態並返回 `null`。
  - [ ] 檢查 `Content-Type` 標頭。如果不是 HTML，記錄類型並返回 `null`。
  - [ ] 使用 `response.text()` 獲取 HTML 內容。
  - [ ] 為提取實現內部 `try...catch`：
    - [ ] 呼叫 `await articleExtractor.extract(htmlContent)`。
    - [ ] 檢查結果（`article?.content`）是否為有效文字。如果是，記錄成功並返回文字。
    - [ ] 如果提取失敗或內容為空，記錄原因並返回 `null`。
    - [ ] 在提取的 `catch` 塊中，記錄錯誤並返回 `null`。
- [ ] 將可選的環境變數 `SCRAPE_TIMEOUT_MS` 和 `SCRAPER_USER_AGENT` 添加到 `.env.example`。

## 測試需求

**指導:** 使用以下測試驗證實現是否符合驗收標準。

- **單元測試:** [915]
  - 為 `src/scraper/articleScraper.ts` 編寫單元測試。[919]
  - 模擬原生 `Workspace`。測試不同場景：
    - 成功擷取（200 OK，HTML 內容類型）-> 模擬 `articleExtractor` 成功 -> 驗證返回的文字 [239]。
    - 成功擷取 -> 模擬 `articleExtractor` 失敗/空內容 -> 驗證 `null` 返回和日誌 [239, 240]。
    - 擷取返回非正常狀態（例如，404, 500）-> 驗證 `null` 返回和日誌 [237, 240]。
    - 擷取返回非 HTML 內容類型 -> 驗證 `null` 返回和日誌 [237, 240]。
    - 擷取拋出網路錯誤/超時 -> 驗證 `null` 返回和日誌 [237, 240]。
  - 模擬 `@extractus/article-extractor` 以模擬成功和失敗情況。[918]
  - 驗證 `Workspace` 是否使用正確的 URL、User-Agent 和超時信號呼叫 [236]。
- **整合測試:** 不適用於此模組本身。[921]
- **手動/CLI 驗證:** 通過故事 3.2 執行間接測試，通過故事 3.4 階段測試器直接測試。[912]
- _(提示：請參閱 `docs/testing-strategy.md` [907-950] 以了解整體方法）。_

## 故事總結（代理在執行後填寫）

- **使用的代理模型:** `<Agent Model Name/Version>`
- **完成說明:** {實現了具有 @extractus/article-extractor 和穩健錯誤處理的擷取模組。}
- **變更日誌:**
  - 初始草稿
```

---

**檔案: ai/stories/3.2.story.md**

```markdown
# 故事 3.2: 將文章擷取整合到主要工作流程中

**狀態:** 草稿

## 目標與背景

**使用者故事:** 作為一名開發者，我希望將文章擷取器整合到主要工作流程中（`src/core/pipeline.ts`），嘗試為每個擁有有效 URL 的 HN 故事擷取文章，在擷取其資料之後。[241]

**背景:** 此故事將擷取器模組（來自故事 3.1 的 `articleScraper.ts`）連接到主要應用程式管道（`src/core/pipeline.ts`），該管道是在 Epic 2 中開發的。它修改了遍歷擷取的故事（這些故事包含在故事 2.2 中加載的資料）的主要迴圈，以便在擁有文章 URL 的故事中包含對 `scrapeArticle` 的調用。然後，將結果（擷取的文字或 null）存儲在記憶體中，與故事物件關聯。[47, 78, 79]

## 詳細需求

- 修改 `src/core/pipeline.ts` 中的主要執行流程（假設邏輯在故事 2.2 中移至此處）。[242]
- 從 `src/scraper/articleScraper.ts` 匯入 `scrapeArticle` 函式。[243] 匯入日誌記錄器。
- 在遍歷擷取的 `Story` 物件的主要迴圈中（在故事 2.2 中擷取註釋之後，並在故事 2.3 中持久化之前）：
  - 檢查 `story.articleUrl` 是否存在且似乎是有效的 HTTP/HTTPS URL。簡單檢查是否以 `http://` 或 `https://` 開頭即可。[243, 244]
  - 如果 URL 缺失或無效，則記錄警告（使用日誌記錄器）並跳過此故事的擷取步驟（例如，在 Epic 4 中進行摘要，或在故事 3.3 中持久化）。並將擷取內容的內部佔位符設置為 `null`。[245]
  - 如果存在有效的 URL：
    - 記錄（"嘗試擷取來自 {story.articleUrl} 的故事 {storyId} 的文章"）。[246]
    - 調用 `await scrapeArticle(story.articleUrl)`。[247]
    - 將結果（擷取的文字字串或 `null`）存儲在記憶體中，與故事物件關聯。在 `src/types/hn.ts` 中為 `Story` 類型定義/添加屬性 `articleContent: string | null`。[247, 513]
    - 清楚地使用日誌記錄器記錄結果（例如，"成功擷取故事 {storyId} 的文章"，"擷取故事 {storyId} 的文章失敗"）。[248]

## 驗收標準（ACs）

- AC1: 執行 `npm run dev` 會執行 Epic 1 和 2 的步驟，然後嘗試在主要管道迴圈中擷取擁有有效 `articleUrl` 的故事。[249]
- AC2: 據有缺失或無效 `articleUrl` 的故事會被擷取步驟跳過，並通過日誌記錄器記錄相應的警告消息。[250]
- AC3: 對於擁有有效 URL 的故事，會使用正確的 URL 調用 `src/scraper/articleScraper.ts` 中的 `scrapeArticle` 函式。[251]
- AC4: 日誌（通過日誌記錄器）清楚地指示每個相關故事的擷取嘗試的開始（"嘗試擷取..."）和成功/失敗結果。[252]
- AC5: 在此階段後，記憶體中保留的故事物件包含 `articleContent` 屬性，該屬性保存擷取的文字（字串）或 `null`（如果擷取被跳過或失敗）。[253]（通過除錯/日誌驗證）。
- AC6: `src/types/hn.ts` 中的 `Story` 類型定義已更新，以包括 `articleContent: string | null` 欄位。[513]

## 技術實現背景

**指導:** 使用以下細節進行實現。如有需要，請參考鏈接的 `docs/` 檔案以獲取更廣泛的背景。

- **相關檔案:**
  - 要修改的檔案：`src/core/pipeline.ts`、`src/types/hn.ts`。
  - _(提示：請參閱 `docs/project-structure.md` [818, 821])._
- **關鍵技術:**
  - TypeScript [846]、Node.js 22.x [851]。
  - 使用 `articleScraper.scrapeArticle`（故事 3.1）、`logger`（故事 1.4）。
  - _(提示：請參閱 `docs/tech-stack.md` [839-905]）。_
- **API 互動/SDK 使用:**
  - 呼叫內部 `scrapeArticle(url)`。
- **資料結構:**
  - 操作在 Epic 2 中擷取的 `Story[]` 上。
  - 擴展 `src/types/hn.ts` 中的 `Story` 介面，以包括 `articleContent: string | null`。[513]
  - 檢查 `story.articleUrl`。
  - _(提示：請參閱 `docs/data-models.md` [506-517]）。_
- **環境變數:**
  - 不直接使用，但 `scrapeArticle` 可能會使用它們（故事 3.1）。
  - _(提示：請參閱 `docs/environment-vars.md` [548-638]）。_
- **程式碼標準說明:**
  - 在呼叫擷取器之前執行 URL 檢查。[244]
  - 清楚地記錄跳過、嘗試、成功、失敗的擷取。[245, 246, 248]
  - 確保 `articleContent` 屬性始終設置（要麼是結果字串，要麼明確設置為 `null`）。

## 任務/子任務

- [ ] 更新 `src/types/hn.ts` 中的 `Story` 類型，以包括 `articleContent: string | null`。
- [ ] 修改 `src/core/pipeline.ts` 中處理每個故事的主要迴圈。
- [ ] 從 `src/scraper/articleScraper.ts` 匯入 `scrapeArticle`。
- [ ] 匯入 `logger`。
- [ ] 在迴圈內（擷取註釋之後，持久化步驟之前）：
  - [ ] 檢查 `story.articleUrl` 是否存在且以 `http` 開頭。
  - [ ] 如果無效/缺失：
    - [ ] 記錄警告消息。
    - [ ] 設置 `story.articleContent = null`。
  - [ ] 如果有效：
    - [ ] 記錄嘗試消息。
    - [ ] 調用 `const scrapedContent = await scrapeArticle(story.articleUrl)`。
    - [ ] 設置 `story.articleContent = scrapedContent`。
    - [ ] 記錄成功（如果 `scrapedContent` 不是 null）或失敗（如果 `scrapedContent` 是 null）。
- [ ] 添加臨時日誌記錄或使用除錯器驗證故事物件中的 `articleContent` 屬性（AC5）。

## 測試需求

**指導:** 使用以下測試驗證實現是否符合驗收標準。

- **單元測試:** [915]
  - 單元測試 `src/core/pipeline.ts` 中修改的管道邏輯。[916]
  - 模擬 `scrapeArticle` 函式。[918]
  - 提供具有和不具有有效 `articleUrl` 的模擬 `Story` 物件。
  - 驗證僅對具有有效 URL 的故事調用 `scrapeArticle` [251]。
  - 驗證傳遞給 `scrapeArticle` 的 URL 正確。
  - 驗證 `scrapeArticle` 的返回值（模擬的文字或模擬的 null）是否正確分配給 `story.articleContent` 屬性 [253]。
  - 驗證根據 URL 有效性和模擬 `scrapeArticle` 結果，調用的日誌（跳過警告、嘗試、成功/失敗）是否正確 [250, 252]。
- **整合測試:** 在這裡重點較少；故事 3.4 提供了更好的擷取整合測試。[921]
- **手動/CLI 驗證:** [912]
  - 執行 `npm run dev`。
  - 檢查日誌中是否有 "嘗試擷取..."、"成功擷取..."、"擷取失敗..." 和 "跳過擷取..." 消息 [250, 252]。
  - 使用除錯器或臨時日誌驗證在管道運行過程中或之後檢查 `story.articleContent` 值 [253]。
- _(提示：請參閱 `docs/testing-strategy.md` [907-950] 以了解整體方法)。_

## 故事總結（代理在執行後填寫）

- **使用的代理模型:** `<Agent Model Name/Version>`
- **完成說明:** {整合擷取器調用到管道中。更新故事類型。驗證處理有效/無效 URL 的邏輯。}
- **變更日誌:**
  - 初始草稿
```

---

**檔案: ai/stories/3.3.story.md**

```markdown
# 故事 3.3: 在本地持久化擷取的文章文字

**狀態:** 草稿

## 目標與背景

**使用者故事:** 作為一名開發者，我希望將成功擷取的文章文字保存到每個故事的單獨本地檔案中，以便在摘要階段將文字內容作為輸入。[254]

**背景:** 此故事為在故事 3.2 中擷取的文章內容添加持久化步驟。在成功擷取（`story.articleContent` 不為 null）之後，這個邏輯將純文字內容寫入 `.txt` 檔案（`{storyId}_article.txt`）中，該檔案位於 Epic 1 中創建的日期標記輸出目錄內。這確保了即使在分階段運行主腳本或需要重新啟動的情況下，擷取的文字也可用於下一階段（摘要 - Epic 4）。如果擷取失敗或被跳過，則不應創建任何檔案。[49, 734, 735]

## 詳細需求

- 如果 `src/core/pipeline.ts` 中尚未存在，請匯入 Node.js `fs`（`writeFileSync`）和 `path` 模組。[255] 匯入日誌記錄器。
- 在主要工作流程中（`src/core/pipeline.ts`），在處理每個故事的迴圈中，_在_ 擷取嘗試完成後（故事 3.2）：
  - 檢查 `story.articleContent` 是否為非空字串（非 null）。
  - 如果是（擷取成功並產生內容）：
    - 獲取當前日期標記的輸出目錄的完整路徑（可從設置中獲得）。[256]
    - 構建檔案名稱：`{storyId}_article.txt`。[257]
    - 使用 `path.join()` 構建完整檔案路徑。[257]
    - 獲取成功擷取的文章文字字串（`story.articleContent`）。[258]
    - 使用 `fs.writeFileSync(fullPath, story.articleContent, 'utf-8')` 將文字保存到檔案中。[259] 將此調用包裹在 `try...catch` 塊中以處理檔案系統錯誤。[260]
    - 記錄成功保存檔案的消息（例如，"已將擷取的文章文字保存到 {filename}"）或遇到的任何檔案寫入錯誤，使用日誌記錄器。[260]
  - 如果 `story.articleContent` 為 null 或空，則確保*不為此故事創建* `_article.txt` 檔案。[261]

## 驗收標準（ACs）

- AC1: 在運行 `npm run dev` 後，日期標記的輸出目錄僅包含在管道運行中，因擷取成功且返回非空文字內容而產生的 `_article.txt` 檔案。[262]
- AC2: 每個文章文字檔案的名稱為 `{storyId}_article.txt`。[263]
- AC3: 每個現有的 `_article.txt` 檔案的內容為 `story.articleContent` 中存儲的純文字字串。[264]
- AC4: 日誌確認每個 `_article.txt` 檔案的成功寫入或報告特定的檔案寫入錯誤。[265]
- AC5: 不會創建空的 `_article.txt` 檔案。僅在擷取成功並返回內容時才會存在檔案。[266]

## 技術實現背景

**指導:** 使用以下細節進行實現。如有需要，請參考鏈接的 `docs/` 檔案以獲取更廣泛的背景。

- **相關檔案:**
  - 要修改的檔案：`src/core/pipeline.ts`。
  - _(提示：請參閱 `docs/project-structure.md` [818])._
- **關鍵技術:**
  - TypeScript [846]、Node.js 22.x [851]。
  - 原生 `fs` 模組（`writeFileSync`）。[259]
  - 原生 `path` 模組（`join`）。[257]
  - 使用 `logger`（故事 1.4）。
  - 使用輸出目錄路徑（來自故事 1.4 邏輯）。
  - 使用 `story.articleContent`（在故事 3.2 中填充）。
  - _(提示：請參閱 `docs/tech-stack.md` [839-905]）。_
- **API 互動/SDK 使用:**
  - `fs.writeFileSync(fullPath, articleContentString, 'utf-8')`。[259]
- **資料結構:**
  - 檢查 `story.articleContent`（字串 | null）。
  - 定義輸出檔案格式 `{storyId}_article.txt` [541]。
  - _(提示：請參閱 `docs/data-models.md` [506-517, 541])._
- **環境變數:**
  - 依賴於 `OUTPUT_DIR_PATH` 可用（來自故事 1.2/1.4）。
  - _(提示：請參閱 `docs/environment-vars.md` [548-638]）。_
- **程式碼標準說明:**
  - 在知道故事的擷取結果後立即放置檔案寫入邏輯。
  - 使用清晰的 `if (story.articleContent)` 檢查。[256]
  - 對 `fs.writeFileSync` 使用 `try...catch`。[260]
  - 清楚地記錄成功/失敗。[260]

## 任務/子任務

- [ ] 在 `src/core/pipeline.ts` 中，確保匯入了 `fs` 和 `path`。確保匯入了日誌記錄器。
- [ ] 確保在故事處理迴圈中可用輸出目錄路徑。
- [ ] 在迴圈內，當 `story.articleContent` 被設置（來自故事 3.2）之後：
  - [ ] 添加 `if (story.articleContent)` 條件。
  - [ ] 在 `if` 區塊內：
    - [ ] 構建檔案名稱：`{storyId}_article.txt`。
    - [ ] 使用 `path.join` 構建完整路徑。
    - [ ] 實現 `try...catch`：
      - [ ] `try`：呼叫 `fs.writeFileSync(fullPath, story.articleContent, 'utf-8')`。
      - [ ] `try`：記錄成功消息。
      - [ ] `catch`：記錄錯誤消息。

## 測試需求

**指導:** 使用以下測試驗證實現是否符合驗收標準。

- **單元測試:** [915]
  - 難以有效地單元測試檔案系統寫入。重點測試管道函式中的*條件邏輯*。[918]
  - 模擬 `fs.writeFileSync`。提供模擬的 `Story` 物件，其中 `articleContent` 有時是字串，有時是 null。
  - 驗證僅在 `articleContent` 為非空字串時調用 `fs.writeFileSync`。[262]
  - 驗證它是否使用正確的路徑（`path.join(outputDir, storyId + '_article.txt')`）和內容（`story.articleContent`）進行調用。[263, 264]
- **整合測試:** [921]
  - 使用 `mock-fs` 或臨時目錄設置/拆解。[924]
  - 運行負責擷取（模擬）和保存的管道段。
  - 驗證僅為模擬擷取返回文字的故事創建檔案。
  - 驗證檔案內容與模擬的文字匹配。
- **手動/CLI 驗證:** [912]
  - 執行 `npm run dev`。
  - 檢查 `output/YYYY-MM-DD/` 目錄。
  - 檢查哪些 `{storyId}_article.txt` 檔案存在。將其與日誌中對應故事 ID 的成功/失敗擷取消息進行比較。驗證僅在成功擷取時存在檔案（AC1, AC5）。
  - 檢查檔案名稱是否正確（AC2）。
  - 打開幾個現有的 `.txt` 檔案並檢查內容（AC3）。
  - 檢查日誌中是否有檔案保存成功/錯誤消息（AC4）。
- _(提示：請參閱 `docs/testing-strategy.md` [907-950] 以了解整體方法)。_

## 故事總結（代理在執行後填寫）

- **使用的代理模型:** `<Agent Model Name/Version>`
- **完成說明:** {添加了條件保存文章文字的邏輯。驗證僅在成功擷取時創建檔案。}
- **變更日誌:**
  - 初始草稿
```

---

**檔案: ai/stories/3.4.story.md**

```markdown
# 故事 3.4: 實現擷取的階段測試工具

**狀態:** 草稿

## 目標與背景

**使用者故事:** 作為一名開發者，我希望有一個單獨的腳本/命令來測試文章擷取邏輯，使用來自本地檔案的 HN 故事資料，允許獨立測試和除錯擷取器。[267]

**背景:** 此故事實現了 Epic 3 的獨立階段測試工具，正如 PRD [736, 764] 所要求的。它創建了 `src/stages/scrape_articles.ts`，該檔案從 Epic 2 中生成的 `{storyId}_data.json` 檔案（或通過 `stage:fetch` 獲得）中讀取故事資料（特別是 URL），為每個 URL 調用 `scrapeArticle` 函式（來自故事 3.1），並將任何成功擷取的文字持久化到 `{storyId}_article.txt` 檔案中（複製故事 3.3 的邏輯）。這允許在不運行完整管道或 HN 擷取階段的情況下，使用先前擷取的故事列表對擷取功能進行測試。[57, 63, 820, 912, 930]

## 詳細需求

- 創建一個新的獨立腳本檔案：`src/stages/scrape_articles.ts`。[268]
- 匯入必要的模組：`fs`（例如，`readdirSync`、`readFileSync`、`writeFileSync`、`existsSync`、`statSync`）、`path`、`logger`（故事 1.4）、`config`（故事 1.2）、`scrapeArticle`（故事 3.1）、日期工具（故事 1.4）。[269]
- 腳本應該：
  - 初始化日誌記錄器。[270]
  - 加載配置（以獲取 `OUTPUT_DIR_PATH`）。[271]
  - 確定目標日期標記目錄路徑（例如，通過當前日期使用日期工具，或稍後可能允許通過 CLI 參數覆蓋 - 當前日期默認值現在很好）。[271] 確保此基礎輸出目錄存在。記錄目標目錄。
  - 檢查目標日期標記目錄是否存在。如果不存在，記錄錯誤並退出（"目錄 {path} 未找到。首先運行擷取階段？"）。
  - 讀取目錄內容並識別所有以 `_data.json` 結尾的檔案。[272] 使用 `fs.readdirSync` 和過濾。
  - 對於找到的每個 `_data.json` 檔案：
    - 構建完整路徑並使用 `fs.readFileSync` 讀取其內容。[273]
    - 解析 JSON 內容。優雅地處理潛在的解析錯誤（記錄錯誤，跳過檔案）。[273]
    - 從解析的資料中提取 `storyId` 和 `articleUrl`。[274]
    - 如果存在有效的 `articleUrl`（以 `http` 開頭）：[274]
      - 記錄嘗試："嘗試擷取來自 {storyId} 的文章，來自 {url}..."。
      - 調用 `await scrapeArticle(articleUrl)`。[274]
      - 如果擷取成功（返回非 null 字串）：
        - 構建輸出檔案名稱 `{storyId}_article.txt`。[275]
        - 構建完整輸出路徑。[275]
        - 使用 `fs.writeFileSync` 將文字寫入檔案（複製故事 3.3 的邏輯，包括 try/catch 和日誌）。[275] 如果檔案存在則覆蓋。[276]
        - 記錄成功結果。
      - 如果擷取失敗（`scrapeArticle` 返回 null）：
        - 記錄失敗結果。
    - 如果 `articleUrl` 缺失或無效：
      - 記錄跳過消息。
  - 記錄總體完成："擷取階段完成處理 {N} 個資料檔案。"。
- 在 `package.json` 中的 `scripts` 部分添加新腳本命令：`"stage:scrape": "ts-node src/stages/scrape_articles.ts"`。[277]

## 驗收標準（ACs）

- AC1: 檔案 `src/stages/scrape_articles.ts` 存在。[279]
- AC2: 在 `package.json` 的 `scripts` 部分定義了腳本 `stage:scrape`。[280]
- AC3: 運行 `npm run stage:scrape`（假設存在來自先前擷取運行的日期標記目錄和 `_data.json` 檔案）成功讀取這些 JSON 檔案。[281]
- AC4: 腳本對於在 JSON 檔案中找到的有效 `articleUrl` 調用了 `scrapeArticle`。[282]
- AC5: 腳本在*相同* 日期標記目錄中創建或更新 `{storyId}_article.txt` 檔案，僅對於成功擷取的文章。[283]
- AC6: 腳本記錄其操作（讀取檔案、嘗試擷取、跳過、保存結果/失敗）針對每個基於找到的 `_data.json` 檔案處理的故事 ID。[284]
- AC7: 腳本僅基於本地 `_data.json` 檔案作為輸入，通過 `scrapeArticle` 擷取外部文章 URL；不調用 Algolia HN API 客戶端。[285, 286]

## 技術實現背景

**指導:** 使用以下細節進行實現。如有需要，請參考鏈接的 `docs/` 檔案以獲取更廣泛的背景。

- **相關檔案:**
  - 要創建的檔案：`src/stages/scrape_articles.ts`。
  - 要修改的檔案：`package.json`。
  - _(提示：請參閱 `docs/project-structure.md` [820] 以了解階段執行器位置)。_
- **關鍵技術:**
  - TypeScript [846]、Node.js 22.x [851]、`ts-node`。
  - 原生 `fs` 模組（`readdirSync`、`readFileSync`、`writeFileSync`、`existsSync`、`statSync`）。[269]
  - 原生 `path` 模組。[269]
  - 使用 `logger`（故事 1.4）、`config`（故事 1.2）、日期工具（故事 1.4）、`scrapeArticle`（故事 3.1）、持久化邏輯（故事 3.3）。
  - _(提示：請參閱 `docs/tech-stack.md` [839-905]）。_
- **API 互動/SDK 使用:**
  - 呼叫內部 `scrapeArticle(url)`。
  - 大量使用 `fs` 模組進行目錄讀取、檔案讀取、檔案寫入。
- **資料結構:**
  - 從 `_data.json` 檔案讀取 JSON 結構 [538-540]。提取 `storyId`、`articleUrl`。
  - 創建 `{storyId}_article.txt` 檔案 [541]。
  - _(提示：請參閱 `docs/data-models.md` )。_
- **環境變數:**
  - 通過 `config.ts` 讀取 `OUTPUT_DIR_PATH`。`scrapeArticle` 可能會使用其他變數。
  - _(提示：請參閱 `docs/environment-vars.md` [548-638]）。_
- **程式碼標準說明:**
  - 清晰地構建腳本（設置、讀取資料檔案、迴圈、處理/擷取/保存）。
  - 對 `scrapeArticle` 使用 `async/await`。
  - 實現對檔案 IO（讀取目錄、讀取檔案、解析 JSON、寫入檔案）的穩健錯誤處理，使用 `try...catch` 和日誌記錄。
  - 使用日誌記錄器進行詳細的進度報告。[284]
  - 將主要邏輯包裹在 async IIFE 或主函式中。

## 任務/子任務

- [ ] 創建 `src/stages/scrape_articles.ts`。
- [ ] 添加匯入：`fs`、`path`、`logger`、`config`、`scrapeArticle`、日期工具。
- [ ] 實現設置：初始化日誌記錄器、加載配置、獲取輸出路徑、獲取目標日期標記路徑。
- [ ] 檢查目標日期標記目錄是否存在，如果不存在，記錄錯誤並退出。
- [ ] 使用 `fs.readdirSync` 獲取目標目錄中的檔案列表。
- [ ] 過濾列表以僅獲取以 `_data.json` 結尾的檔案。
- [ ] 迴圈遍歷 `_data.json` 檔案名稱：
  - [ ] 為 JSON 檔案構建完整路徑。
  - [ ] 使用 `try...catch` 讀取和解析 JSON 檔案：
    - [ ] `try`：讀取檔案（`fs.readFileSync`）。解析 JSON（`JSON.parse`）。
    - [ ] `catch`：記錄錯誤（讀取/解析），繼續到下一個檔案。
  - [ ] 提取 `storyId` 和 `articleUrl`。
  - [ ] 檢查 `articleUrl` 是否有效（以 `http` 開頭）。
  - [ ] 如果有效：
    - [ ] 記錄嘗試。
    - [ ] 呼叫 `content = await scrapeArticle(articleUrl)`。
    - [ ] `if (content)`：
      - [ ] 構建 `.txt` 輸出路徑。
      - [ ] 使用 `try...catch` 寫入檔案（`fs.writeFileSync`）。記錄成功/錯誤。
    - [ ] `else`：記錄擷取失敗。
  - [ ] 如果 URL 無效：記錄跳過。
- [ ] 記錄完成消息。
- [ ] 在 `package.json` 中添加 `"stage:scrape": "ts-node src/stages/scrape_articles.ts"`。

## 測試需求

**指導:** 使用以下測試驗證實現是否符合驗收標準。

- **單元測試:** 難以有效地單元測試整個腳本。重點單元測試核心 `scrapeArticle` 模組（故事 3.1）和工具。[915]
- **整合測試:** N/A 對於腳本本身。[921]
- **手動/CLI 驗證（主要測試方法）：** [912, 927, 930]
  - 確保存在來自 `npm run stage:fetch` 或 `npm run dev` 的 `_data.json` 檔案。
  - 運行 `npm run stage:scrape`。[281]
  - 驗證成功執行。
  - 檢查日誌中有關讀取檔案、跳過、嘗試擷取、成功/失敗消息和保存消息的記錄 [284]。
  - 檢查 `output/YYYY-MM-DD/` 目錄中新創建/更新的 `{storyId}_article.txt` 檔案。驗證它們與日誌中擷取成功的故事相對應 [283, 285]。
  - 驗證腳本僅基於本地檔案執行擷取操作（AC7）。
  - 修改 `package.json` 以添加腳本（AC2）。
- _(提示：請參閱 `docs/testing-strategy.md` [907-950]，其中將階段執行器識別為驗收測試的關鍵部分）。_

## 故事總結（代理在執行後填寫）

- **使用的代理模型:** `<Agent Model Name/Version>`
- **完成說明:** {實現階段執行器。讀取 \_data.json，呼叫擷取器，條件性保存 \_article.txt。更新 package.json。}
- **變更日誌:**
  - 初始草稿
```

---

## **結束 Epic 3 的報告**
