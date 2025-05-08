好的，繼續以 **SM 模式**。

生成 **Epic 2: HN 數據獲取與持久化** 的故事。這假設 Epic 1 的基礎工作（專案初始化、通過 `src/utils/config.ts` 進行配置設置、通過 `src/utils/logger.ts` 設置日誌記錄器，以及在 `src/index.ts` 中建立輸出目錄邏輯）已完成並可供使用。

---

## **故事報告：Epic 2**

**檔案: ai/stories/2.1.story.md**

```markdown
# 故事 2.1: 實現 Algolia HN API 客戶端

**狀態：** 草稿

## 目標與背景

**使用者故事：** 作為一名開發者，我希望有一個專門的客戶端模組來與 Algolia Hacker News 搜尋 API 互動，以便能夠封裝、重用並使用所需的原生 `Workspace` API 來獲取故事和評論。 [155]

**背景：** 此故事建立了從 Algolia 提供的外部 Hacker News API 獲取數據的主要介面。它將特定的 API 呼叫（`GET /search` 用於故事和評論）和數據提取邏輯封裝到一個可重用的模組中（`src/clients/algoliaHNClient.ts`）。此客戶端將被主要管道（故事 2.2）和階段測試工具（故事 2.4）使用。它基於在 Epic 1 中建立的日誌記錄器（故事 1.4）。 [54, 60, 62, 77]

## 詳細需求

- 建立一個新模組：`src/clients/algoliaHNClient.ts`。 [156]
- 在客戶端中實現一個異步函數 `WorkspaceTopStories`： [157]
  - 使用原生 `Workspace` [749] 來呼叫 Algolia HN 搜尋 API 端點以獲取前頁故事（`http://hn.algolia.com/api/v1/search?tags=front_page&hitsPerPage=10`）。 [4, 6, 7, 157] 如有需要，調整 `hitsPerPage` 以確保獲取 10 個故事。
  - 解析 JSON 回應。 [158]
  - 提取每個故事所需的元數據：`objectID`（用作 `storyId`）、`title`、`url`（用作 `articleUrl`）、`points`、`num_comments`。 [159, 522] 優雅地處理可能缺失的 `url` 欄位（使用故事 1.4 的日誌記錄器記錄警告，將其視為 null）。 [160]
  - 為每個故事構建 `hnUrl`（例如，`https://news.ycombinator.com/item?id={storyId}`）。 [161]
  - 返回一個結構化故事物件的陣列（定義一個 `Story` 類型，可能在 `src/types/hn.ts` 中）。 [162, 506-511]
- 在客戶端中實現一個單獨的異步函數 `WorkspaceCommentsForStory`： [163]
  - 接受 `storyId`（字串）和 `maxComments` 限制（數字）作為參數。 [163]
  - 使用原生 `Workspace` 來呼叫 Algolia HN 搜尋 API 端點以獲取特定故事的評論（`http://hn.algolia.com/api/v1/search?tags=comment,story_{storyId}&hitsPerPage={maxComments}`）。 [12, 13, 14, 164]
  - 解析 JSON 回應。 [165]
  - 提取所需的評論數據：`objectID`（用作 `commentId`）、`comment_text`、`author`、`created_at`。 [165, 524]
  - 過濾掉 `comment_text` 為 null 或空的評論。確保僅返回最多 `maxComments` 條評論。 [166]
  - 返回一個結構化評論物件的陣列（定義一個 `Comment` 類型，可能在 `src/types/hn.ts` 中）。 [167, 500-505]
- 實現基本的錯誤處理，使用 `try...catch` 包裹 `Workspace` 呼叫並檢查 `response.ok` 狀態。 [168] 使用 Epic 1 的日誌記錄器工具（故事 1.4）記錄錯誤。 [169]
- 定義 TypeScript 介面/類型以符合 API 回應的預期結構（所需的子集）以及客戶端函數返回的數據（`Story`、`Comment`）。將這些放置在 `src/types/hn.ts` 中。 [169, 821]

## 驗收標準 (ACs)

- AC1：模組 `src/clients/algoliaHNClient.ts` 存在並導出 `WorkspaceTopStories` 和 `WorkspaceCommentsForStory` 函數。 [170]
- AC2：呼叫 `WorkspaceTopStories` 會向正確的 Algolia 端點（`search?tags=front_page&hitsPerPage=10`）發送網絡請求，並返回一個解析為包含指定元數據（`storyId`、`title`、`articleUrl`、`hnUrl`、`points`、`num_comments`）的 10 個 `Story` 物件的陣列的承諾。 [171]
- AC3：使用有效的 `storyId` 和 `maxComments` 限制呼叫 `WorkspaceCommentsForStory` 會向正確的 Algolia 端點（`search?tags=comment,story_{storyId}&hitsPerPage={maxComments}`）發送網絡請求，並返回一個解析為 `Comment` 物件的陣列（最多 `maxComments`），過濾掉空的評論。 [172]
- AC4：這兩個函數在內部都使用原生 `Workspace` API。 [173]
- AC5：網絡錯誤或非成功的 API 回應（例如，狀態 4xx、5xx）會被捕獲並使用故事 1.4 的日誌記錄器記錄。 [174] 函數在失敗情況下應該返回一個空陣列或拋出特定錯誤以供呼叫者處理。
- AC6：相關的 TypeScript 類型（`Story`、`Comment`）在 `src/types/hn.ts` 中定義並在客戶端模組中使用。 [175]

## 技術實施背景

**指導：** 使用以下細節進行實施。如有需要，請參考鏈接的 `docs/` 文件以獲取更廣泛的背景。

- **相關文件：**
  - 要建立的檔案：`src/clients/algoliaHNClient.ts`、`src/types/hn.ts`。
  - 可能需要修改的檔案：`src/types/index.ts`（如果使用 barrel file）。
  - _(提示：請參閱 `docs/project-structure.md` [817, 821] 以了解位置)。_
- **關鍵技術：**
  - TypeScript [846]、Node.js 22.x [851]、原生 `Workspace` API [863]。
  - 使用來自 Epic 1 的 `logger` 工具（故事 1.4）。
  - _(提示：請參閱 `docs/tech-stack.md` [839-905] 以獲取完整列表)。_
- **API 互動 / SDK 使用：**
  - Algolia HN 搜尋 API `GET /search` 端點。 [2]
  - 基本 URL：`http://hn.algolia.com/api/v1` [3]
  - 參數：`tags=front_page`、`hitsPerPage=10`（用於故事）[6, 7]；`tags=comment,story_{storyId}`、`hitsPerPage={maxComments}`（用於評論）[13, 14]。
  - 檢查 `response.ok` 並解析 JSON 回應（`response.json()`）。 [168, 158, 165]
  - 使用 `try...catch` 處理潛在的網絡錯誤。 [168]
  - 不需要身份驗證。 [3]
  - _(提示：請參閱 `docs/api-reference.md` [2-21] 以了解詳細信息)。_
- **數據結構：**
  - 定義 `Comment` 介面：`{ commentId: string, commentText: string | null, author: string | null, createdAt: string }`。 [501-505]
  - 定義 `Story` 介面（初始欄位）：`{ storyId: string, title: string, articleUrl: string | null, hnUrl: string, points?: number, numComments?: number }`。 [507-511]
  - （這些類型將在後續故事中擴展 [512-517]）。
  - 參考 `docs/data-models.md` 中的 Algolia 回應子集架構 [521-525]。
  - _(提示：請參閱 `docs/data-models.md` 以了解完整細節)。_
- **環境變數：**
  - 此客戶端本身不需要直接的環境變數（使用硬編碼的基本 URL，通過參數獲取評論限制）。
  - _(提示：請參閱 `docs/environment-vars.md` [548-638] 以了解所有變數)。_
- **編碼標準說明：**
  - 對於 `Workspace` 呼叫使用 `async/await`。
  - 對於錯誤和重要事件（例如，`url` 缺失的警告）使用日誌記錄器。 [160]
  - 清晰地導出類型和函數。

## 任務 / 子任務

- [ ] 建立 `src/types/hn.ts` 並定義 `Comment` 和初始的 `Story` 介面。
- [ ] 建立 `src/clients/algoliaHNClient.ts`。
- [ ] 導入必要的類型和日誌記錄器工具。
- [ ] 實現 `WorkspaceTopStories` 函數：
  - [ ] 構建用於頂部故事的 Algolia URL。
  - [ ] 使用 `Workspace` 並搭配 `try...catch`。
  - [ ] 檢查 `response.ok`，如果不正常則記錄錯誤。
  - [ ] 解析 JSON 回應。
  - [ ] 將 `hits` 映射為 `Story` 物件，提取所需欄位，處理 null `url`，構建 `hnUrl`。
  - [ ] 返回 `Story` 物件的陣列（或處理錯誤情況）。
- [ ] 實現 `WorkspaceCommentsForStory` 函數：
  - [ ] 接受 `storyId` 和 `maxComments` 參數。
  - [ ] 使用參數構建用於評論的 Algolia URL。
  - [ ] 使用 `Workspace` 並搭配 `try...catch`。
  - [ ] 檢查 `response.ok`，如果不正常則記錄錯誤。
  - [ ] 解析 JSON 回應。
  - [ ] 將 `hits` 映射為 `Comment` 物件，提取所需欄位。
  - [ ] 過濾掉 null/空的 `comment_text`。
  - [ ] 將結果限制為 `maxComments`。
  - [ ] 返回 `Comment` 物件的陣列（或處理錯誤情況）。
- [ ] 根據需要導出函數和類型。

## 測試需求

**指導：** 使用以下測試驗證實現是否符合驗收標準。

- **單元測試：** [915]
  - 為 `src/clients/algoliaHNClient.ts` 編寫單元測試。 [919]
  - 模擬原生 `Workspace` 函數（例如，使用 `jest.spyOn(global, 'fetch')`）。 [918]
  - 測試 `WorkspaceTopStories`：提供模擬的成功回應（符合 Algolia 結構的有效 JSON [521-523]），並驗證正確解析、映射為 `Story` 物件 [171]，以及 `hnUrl` 的構建。測試缺失 `url` 欄位的情況。測試模擬的錯誤回應（網絡錯誤、非正常狀態），並驗證錯誤記錄 [174] 和返回值。
  - 測試 `WorkspaceCommentsForStory`：提供模擬的成功回應 [524-525]，並驗證正確解析、映射為 `Comment` 物件，過濾空評論，並限制為 `maxComments` [172]。測試模擬的錯誤回應並驗證記錄 [174]。
  - 驗證 `Workspace` 是否使用正確的 URL 和參數進行呼叫 [171, 172]。
- **整合測試：** 不適用於此客戶端模組本身，但它將在管道整合測試中使用。 [921]
- **手動/CLI 驗證：** 通過故事 2.2 執行間接測試，並通過故事 2.4 階段執行器直接測試。 [912]
- _(提示：請參閱 `docs/testing-strategy.md` [907-950] 以了解整體方法)。_

## 故事總結（代理在執行後填寫）

- **使用的代理模型：** `<Agent Model Name/Version>`
- **完成說明：** {關於實現選擇、困難或需要後續處理的任何說明}
- **變更記錄：**
  - 初始草稿
```

---

**檔案: ai/stories/2.2.story.md**

```markdown
# 故事 2.2: 整合 HN 數據獲取到主要工作流程

**狀態：** 草稿

## 目標與背景

**使用者故事：** 作為一名開發者，我希望將 HN 數據獲取邏輯整合到主要應用程序工作流程中（`src/index.ts`），以便在完成 Epic 1 的設置後，運行應用程序能夠檢索前 10 個故事及其評論。 [176]

**背景：** 此故事將在故事 2.1 中創建的 HN API 客戶端連接到在 Epic 1 中建立的主要應用程序入口點（`src/index.ts`）（故事 1.3）。它修改主要執行流程，以便在初始設置（日誌記錄器、配置、輸出目錄）之後調用客戶端函數（`WorkspaceTopStories`、`WorkspaceCommentsForStory`）。它使用在故事 1.2 中加載的 `MAX_COMMENTS_PER_STORY` 配置值。獲取的數據（故事及其相關評論）在此階段結束時保存在內存中。 [46, 77]

## 詳細需求

- 修改 `src/index.ts` 中的主要執行流程（或由其調用的主要異步函數，可能將邏輯移動到 `src/core/pipeline.ts`，如 `ARCH` [46, 53] 和 `PS` [818] 所建議）。 **建議：** 建立 `src/core/pipeline.ts` 和一個 `runPipeline` 異步函數，然後從 `src/index.ts` 調用此函數。
- 從故事 2.1 中導入 `algoliaHNClient` 函數（`WorkspaceTopStories`、`WorkspaceCommentsForStory`）。 [177]
- 從配置模組（`src/utils/config.ts`）導入 `MAX_COMMENTS_PER_STORY`。 [177, 563] 另請導入日誌記錄器。
- 在主要管道函數中，在 Epic 1 設置之後（配置加載、日誌記錄器初始化、輸出目錄創建）：
  - 調用 `await fetchTopStories()`。 [178]
  - 記錄獲取的故事數量（例如，"Fetched X stories."）。 [179] 使用來自故事 1.4 的日誌記錄器。
  - 從配置模組檢索 `MAX_COMMENTS_PER_STORY` 值。確保將其解析為數字。如有必要，提供默認值（例如，50，匹配 `ENV` [564]）。
  - 遍歷獲取的 `Story` 物件數組。 [179]
  - 對於每個 `Story`：
    - 記錄進度（例如，"Fetching up to Y comments for story {storyId}..."）。 [182]
    - 調用 `await fetchCommentsForStory()`，傳遞 `story.storyId` 和配置的 `MAX_COMMENTS_PER_STORY` 值。 [180]
    - 將獲取的評論（返回的 `Comment[]`）存儲在相應的 `Story` 物件中（例如，向 `Story` 類型/物件添加 `comments: Comment[]` 屬性）。 [181] 擴展 `src/types/hn.ts` 中的 `Story` 類型定義。 [512]
- 確保適當處理來自客戶端函數的錯誤（例如，記錄錯誤並可能跳過該故事的評論獲取）。

## 驗收標準 (ACs)

- AC1: 執行 `npm run dev` 會執行 Epic 1 設置步驟，然後使用 `algoliaHNClient` 為每個故事獲取故事和評論。 [183]
- AC2: 日誌（通過日誌記錄器）清楚地顯示了獲取故事的開始和成功完成，以及為每個 10 個故事獲取評論的開始。 [184]
- AC3: 配置的 `MAX_COMMENTS_PER_STORY` 值從配置中讀取，解析為數字，並用於呼叫 `WorkspaceCommentsForStory`。 [185]
- AC4: 在成功執行後（在故事 2.3 中持久化之前），內存中持有的 `Story` 物件包含一個 `comments` 屬性，該屬性填充了獲取的 `Comment` 物件的數組。 [186] （通過調試器或臨時日誌驗證）。
- AC5: `src/types/hn.ts` 中的 `Story` 類型定義已更新為包含 `comments: Comment[]` 屬性。 [512]
- AC6: （如果已實現）核心邏輯已移動到 `src/core/pipeline.ts` 並從 `src/index.ts` 調用。 [818]

## 技術實施背景

**指導：** 使用以下細節進行實施。如有需要，請參考鏈接的 `docs/` 文件以獲取更廣泛的背景。

- **相關文件：**
  - 要建立的檔案：`src/core/pipeline.ts`（建議）。
  - 可能需要修改的檔案：`src/index.ts`、`src/types/hn.ts`。
  - _(提示：請參閱 `docs/project-structure.md` [818, 821, 822])._
- **關鍵技術：**
  - TypeScript [846]、Node.js 22.x [851]。
  - 使用 `algoliaHNClient`（故事 2.1）、`config`（故事 1.2）、`logger`（故事 1.4）。
  - _(提示：請參閱 `docs/tech-stack.md` [839-905])._
- **API 互動 / SDK 使用：**
  - 呼叫內部 `algoliaHNClient.fetchTopStories()` 和 `algoliaHNClient.fetchCommentsForStory()`。
- **數據結構：**
  - 在 `src/types/hn.ts` 中擴展 `Story` 介面以包括 `comments: Comment[]`。 [512]
  - 在內存中操作 `Story` 和 `Comment` 物件的數組。
  - _(提示：請參閱 `docs/data-models.md` [500-517])._
- **環境變數：**
  - 通過 `config.ts` 讀取 `MAX_COMMENTS_PER_STORY`。 [177, 563]
  - _(提示：請參閱 `docs/environment-vars.md` [548-638])._
- **編碼標準說明：**
  - 對於呼叫客戶端函數使用 `async/await`。
  - 將獲取邏輯結構化（例如，在循環內）。
  - 對於進度和錯誤報告使用日誌記錄器。 [182, 184]
  - 考慮將主要循環邏輯放入 `src/core/pipeline.ts` 中的 `runPipeline` 函數內。

## 任務 / 子任務

- [ ] （建議）建立 `src/core/pipeline.ts` 並定義一個異步的 `runPipeline` 函數。
- [ ] 修改 `src/index.ts` 以導入和調用 `runPipeline`。將現有的設置邏輯（初始化日誌記錄器、加載配置、創建目錄）移動到 `runPipeline` 中或確保其在之前運行。
- [ ] 在 `pipeline.ts`（或 `index.ts`）中，從 `algoliaHNClient` 導入 `WorkspaceTopStories`、`WorkspaceCommentsForStory`。
- [ ] 導入 `config` 和 `logger`。
- [ ] 在初始設置後調用 `WorkspaceTopStories`。記錄數量。
- [ ] 從配置中獲取 `MAX_COMMENTS_PER_STORY`，確保其為數字。
- [ ] 更新 `src/types/hn.ts` 中的 `Story` 類型以包括 `comments: Comment[]`。
- [ ] 遍歷獲取的故事：
  - [ ] 記錄開始獲取故事 ID 的評論。
  - [ ] 使用 `storyId` 和 `maxComments` 呼叫 `WorkspaceCommentsForStory`。
  - [ ] 處理來自客戶端函數調用的潛在錯誤。
  - [ ] 將返回的評論數組分配給當前故事物件的 `comments` 屬性。
- [ ] 添加臨時日誌或使用調試器驗證內存中的故事包含評論（AC4）。

## 測試需求

**指導：** 使用以下測試驗證實現是否符合驗收標準。

- **單元測試：** [915]
  - 如果邏輯移動到 `src/core/pipeline.ts`，則單元測試 `runPipeline`。 [916]
  - 模擬 `algoliaHNClient` 函數（`WorkspaceTopStories`、`WorkspaceCommentsForStory`）。 [918]
  - 模擬 `config` 以提供 `MAX_COMMENTS_PER_STORY`。
  - 模擬 `logger`。
  - 驗證 `WorkspaceTopStories` 被調用一次。
  - 驗證 `WorkspaceCommentsForStory` 被調用為每個從模擬的 `WorkspaceTopStories` 返回的故事，並且接收到來自配置的正確 `storyId` 和 `maxComments` 值 [185]。
  - 驗證來自模擬的 `WorkspaceCommentsForStory` 的結果正確分配給故事物件的 `comments` 屬性。
- **整合測試：**
  - 可以對提取階段進行整合測試，使用真實的 `algoliaHNClient`（或輕度模擬版本檢查調用）並驗證內存中的數據結構，但這在階段執行器（故事 2.4）中已經涵蓋。 [921]
- **手動/CLI 驗證：**
  - 執行 `npm run dev`。
  - 檢查獲取故事和評論消息的日誌 [184]。
  - 使用調試器或臨時 `console.log` 在管道代碼中檢查故事物件，確認其 `comments` 屬性已填充 [186]。
- _(提示：請參閱 `docs/testing-strategy.md` [907-950] 以了解整體方法)。_

## 故事總結（代理在執行後填寫）

- **使用的代理模型：** `<Agent Model Name/Version>`
- **完成說明：** {邏輯移動到 src/core/pipeline.ts。驗證內存中的數據結構。}
- **變更記錄：**
  - 初始草稿
```

---

**檔案: ai/stories/2.3.story.md**

```markdown
# 故事 2.3: 在本地持久化獲取的 HN 數據

**狀態：** 草稿

## 目標與背景

**使用者故事：** 作為一名開發者，我想將獲取的 HN 故事（包括其評論）保存到帶有日期戳的輸出目錄中的 JSON 檔案中，以便原始數據能夠在後續管道階段和調試中持久化到本地。 [187]

**背景：** 此故事跟隨故事 2.2，其中 HN 數據（帶評論的故事）已被獲取並存儲在內存中。現在，這些數據需要保存到本地文件系統。它使用在 Epic 1 中創建的帶有日期戳的輸出目錄（故事 1.4），並為每個故事寫入一個 JSON 檔案，包含故事元數據及其評論。這些持久化的數據（`{storyId}_data.json`）是後續階段的輸入（抓取 - Epic 3，摘要 - Epic 4，電子郵件組裝 - Epic 5）。 [48, 734, 735]

## 詳細需求

- 定義輸出檔案內容的一致 JSON 結構。 [188] 來自 `docs/data-models.md` [539] 的範例：`{ storyId: "...", title: "...", articleUrl: "...", hnUrl: "...", points: ..., numComments: ..., fetchedAt: "ISO_TIMESTAMP", comments: [{ commentId: "...", commentText: "...", author: "...", createdAt: "...", ... }, ...] }`。包括一個時間戳（`WorkspaceedAt`），表示數據被獲取/保存的時間。 [190]
- 在管道模組（`src/core/pipeline.ts` 或 `src/index.ts`）中導入 Node.js `fs`（特別是 `writeFileSync`）和 `path` 模組。 [190] 導入 `date-fns` 或使用 `new Date().toISOString()` 獲取時間戳。
- 在主要工作流程（`pipeline.ts`）中，在迭代故事的循環內（在故事 2.2 中立即獲取評論並添加到故事物件之後）： [191]
  - 獲取帶有日期戳的輸出目錄的完整路徑（該路徑應該從故事 1.4 的初始設置邏輯中確定/傳遞）。 [191]
  - 生成當前時間戳，格式為 ISO 8601（例如，`new Date().toISOString()`），並將其作為 `WorkspaceedAt` 添加到故事物件中。 [190] 更新 `Story` 類型在 `src/types/hn.ts` 中。 [516]
  - 為故事的數據構建檔案名：`{storyId}_data.json`。 [192]
  - 使用 `path.join()` 構建完整的檔案路徑。 [193]
  - 準備要保存的數據對象，匹配定義的 JSON 結構（包括 `storyId`、`title`、`articleUrl`、`hnUrl`、`points`、`numComments`、`WorkspaceedAt`、`comments`）。
  - 使用 `JSON.stringify(storyData, null, 2)` 將準備好的故事數據對象序列化為可讀的 JSON 字符串。 [194]
  - 使用 `fs.writeFileSync()` 將 JSON 字符串寫入檔案。使用 `try...catch` 塊處理檔案寫入過程中的錯誤。 [195]
  - 使用日誌記錄器記錄每個故事數據檔案成功持久化或檔案寫入過程中遇到的任何錯誤。 [196]

## 驗收標準 (ACs)

- AC1: 在運行 `npm run dev` 後，帶有日期戳的輸出目錄（例如，`./output/YYYY-MM-DD/`）包含正好 10 個名為 `{storyId}_data.json` 的檔案（假設成功獲取了 10 個故事）。 [197]
- AC2: 每個 JSON 檔案包含有效的 JSON，表示單個故事物件，包括其元數據（`storyId`、`title`、`articleUrl`、`hnUrl`、`points`、`numComments`）、一個 `WorkspaceedAt` ISO 時間戳，以及其獲取的 `comments` 數組，匹配 `docs/data-models.md` [538-540] 中定義的結構。 [198]
- AC3: 每個檔案的 `comments` 陣列中的評論數量不超過 `MAX_COMMENTS_PER_STORY`。 [199]
- AC4: 日誌顯示已嘗試將數據保存到檔案中，並報告成功或特定的檔案寫入錯誤。 [200]
- AC5: `src/types/hn.ts` 中的 `Story` 類型定義已更新為包含 `WorkspaceedAt: string` 屬性。 [516]

## 技術實施背景

**指導：** 使用以下細節進行實施。如有需要，請參考鏈接的 `docs/` 文件以獲取更廣泛的背景。

- **相關文件：**
  - 要修改的檔案：`src/core/pipeline.ts`（或 `src/index.ts`）、`src/types/hn.ts`。
  - _(提示：請參閱 `docs/project-structure.md` [818, 821, 822])._
- **關鍵技術：**
  - TypeScript [846]、Node.js 22.x [851]。
  - 原生 `fs` 模組（`writeFileSync`） [190]。
  - 原生 `path` 模組（`join`） [193]。
  - `JSON.stringify` [194]。
  - 使用日誌記錄器（故事 1.4）。
  - 使用故事 1.4 邏輯創建的輸出目錄路徑。
  - _(提示：請參閱 `docs/tech-stack.md` [839-905])._
- **API 互動 / SDK 使用：**
  - `fs.writeFileSync(filePath, jsonDataString, 'utf-8')`。 [195]
- **數據結構：**
  - 使用 `Story` 和 `Comment` 類型來自 `src/types/hn.ts`。
  - 擴展 `Story` 類型以包括 `WorkspaceedAt: string`。 [516]
  - 創建的 JSON 結構匹配 `docs/data-models.md` 中的 `{storyId}_data.json` 架構。 [538-540]
  - _(提示：請參閱 `docs/data-models.md`)._
- **環境變數：**
  - 直接不需要，依賴於從配置（故事 1.2）可用的 `OUTPUT_DIR_PATH`，由目錄創建邏輯（故事 1.4）使用。
  - _(提示：請參閱 `docs/environment-vars.md` [548-638])._
- **編碼標準說明：**
  - 對於 `writeFileSync` 調用使用 `try...catch`。 [195]
  - 對於可讀性使用帶縮進的 `JSON.stringify`（`null, 2`）。 [194]
  - 清晰地使用日誌記錄器記錄成功/失敗。 [196]

## 任務 / 子任務

- [ ] 在 `pipeline.ts`（或 `index.ts`）中導入 `fs` 和 `path`。
- [ ] 更新 `src/types/hn.ts` 中的 `Story` 類型以包括 `WorkspaceedAt: string`。
- [ ] 確保帶有日期戳的輸出目錄的完整路徑在故事處理循環內可用。
- [ ] 在循環內（在為故事獲取評論之後）：
  - [ ] 獲取當前 ISO 時間戳（`new Date().toISOString()`）。
  - [ ] 將時間戳添加到故事物件作為 `WorkspaceedAt`。
  - [ ] 構建輸出檔案名：`{storyId}_data.json`。
  - [ ] 使用 `path.join(outputDirPath, filename)` 構建完整檔案路徑。
  - [ ] 創建匹配指定 JSON 結構的數據對象，包括評論。
  - [ ] 使用 `JSON.stringify(data, null, 2)` 序列化數據對象。
  - [ ] 使用 `try...catch` 塊：
    - [ ] 在 `try` 內：調用 `fs.writeFileSync(fullPath, jsonString, 'utf-8')`。
    - [ ] 在 `try` 內：記錄成功消息和檔案名。
    - [ ] 在 `catch` 內：記錄檔案寫入錯誤和檔案名。

## 測試需求

**指導：** 使用以下測試驗證實現是否符合驗收標準。

- **單元測試：** [915]
  - 直接在單元測試中測試檔案系統交互可能會很脆弱。 [918]
  - 專注於數據準備邏輯的單元測試：確保在 `JSON.stringify` 之前創建的對象具有正確的結構（`storyId`、`title`、`articleUrl`、`hnUrl`、`points`、`numComments`、`WorkspaceedAt`、`comments`），基於示例輸入 `Story` 物件。 [920]
  - 驗證 `WorkspaceedAt` 時間戳是否正確添加。
- **整合測試：** [921]
  - 可以使用 `mock-fs` 或在臨時目錄中進行實際檔案系統寫入的整合測試（在設置期間創建，在拆卸期間刪除）。 [924]
  - 驗證生成的檔案名和寫入的內容是否與預期的 JSON 結構 [538-540] 和內容匹配。
- **手動/CLI 驗證：** [912]
  - 執行 `npm run dev`。
  - 檢查帶有當前日期的 `output/YYYY-MM-DD/` 目錄。
  - 驗證存在 10 個名為 `{storyId}_data.json` 的檔案（AC1）。
  - 打開幾個檔案，從視覺上檢查 JSON 結構，檢查所有必需欄位（元數據、`WorkspaceedAt`、`comments` 陣列），並驗證評論數量 <= `MAX_COMMENTS_PER_STORY`（AC2、AC3）。
  - 檢查日誌控制台以獲取檔案寫入的成功消息或任何錯誤（AC4）。
- _(提示：請參閱 `docs/testing-strategy.md` [907-950] 以了解整體方法)。_

## 故事總結（代理在執行後填寫）

- **使用的代理模型：** `<Agent Model Name/Version>`
- **完成說明：** {檔案成功保存到 ./output/YYYY-MM-DD/ 目錄。}
- **變更記錄：**
  - 初始草稿
```

---

**檔案: ai/stories/2.4.story.md**

```markdown
# 故事 2.4: 實現 HN 獲取的階段測試工具

**狀態：** 草稿

## 目標與背景

**使用者故事：** 作為一名開發者，我想要一個單獨的可執行腳本，_僅僅_ 執行 HN 數據獲取和持久化，這樣我就可以獨立於完整的管道測試和觸發這個階段。 [201]

**背景：** 此故事滿足 PRD 要求 [736] 中針對特定階段的測試工具 [764]。它創建了一個獨立的 Node.js 腳本（`src/stages/fetch_hn_data.ts`），該腳本複製了故事 2.1、2.2（部分）和 2.3 的核心邏輯。此腳本將初始化必要的組件（日誌記錄器、配置），調用 `algoliaHNClient` 獲取故事和評論，並將結果持久化到帶有日期戳的輸出目錄，就像主要管道在此時所做的那樣。這允許在不運行後續抓取、摘要或電子郵件階段的情況下，對 Algolia API 交互和數據持久化進行獨立測試。 [57, 62, 912]

## 詳細需求

- 創建一個新的獨立腳本檔案：`src/stages/fetch_hn_data.ts`。 [202]
- 此腳本應執行此階段所需的基本設置：
  - 初始化日誌記錄工具（來自故事 1.4）。 [203]
  - 使用配置工具（來自故事 1.2）加載配置以獲取 `MAX_COMMENTS_PER_STORY` 和 `OUTPUT_DIR_PATH`。 [203]
  - 使用故事 1.4 中的工具確定當前日期（`YYYY-MM-DD`）。 [203]
  - 構建帶有日期戳的輸出目錄路徑。 [203]
  - 確保輸出目錄存在（如果不存在，則遞歸創建，重用故事 1.4 中的邏輯/工具）。 [203]
- 然後，該腳本應執行獲取和持久化的核心邏輯：
  - 導入並使用 `algoliaHNClient.fetchTopStories` 和 `algoliaHNClient.fetchCommentsForStory`（來自故事 2.1）。 [204]
  - 導入 `fs` 和 `path`。
  - 複製故事 2.2 中的獲取循環邏輯（獲取故事，然後循環獲取每個故事的評論，使用加載的 `MAX_COMMENTS_PER_STORY` 限制）。 [204]
  - 複製故事 2.3 中的持久化邏輯（添加 `WorkspaceedAt` 時間戳，準備數據對象，`JSON.stringify`，`fs.writeFileSync` 到帶有日期戳的目錄中的 `{storyId}_data.json`）。 [204]
- 該腳本應使用日誌記錄工具記錄其進度（例如，"Starting HN data fetch stage..."、"Fetching stories..."、"Fetching comments for story X..."、"Saving data for story X..."）。 [205]
- 在 `package.json` 的 `"scripts"` 下添加一個新的腳本命令：`"stage:fetch": "ts-node src/stages/fetch_hn_data.ts"`。 [206]

## 驗收標準 (ACs)

- AC1: 檔案 `src/stages/fetch_hn_data.ts` 存在。 [207]
- AC2: 腳本 `stage:fetch` 在 `package.json` 的 `scripts` 部分中定義。 [208]
- AC3: 執行 `npm run stage:fetch` 成功執行，僅執行設置（日誌記錄器、配置、輸出目錄）、獲取（故事、評論）和持久化步驟（到 JSON 檔案）。 [209]
- AC4: 執行 `npm run stage:fetch` 在正確的帶有日期戳的輸出目錄中創建相同的 10 個 `{storyId}_data.json` 檔案，與運行主要的 `npm run dev` 命令相同（直到 Epic 2 功能結束）。 [210]
- AC5: `npm run stage:fetch` 生成的日誌僅反映獲取和持久化步驟，而不是後續的管道階段（抓取、摘要、電子郵件）。 [211]

## 技術實施背景

**指導：** 使用以下細節進行實施。如有需要，請參考鏈接的 `docs/` 文件以獲取更廣泛的背景。

- **相關文件：**
  - 要創建的檔案：`src/stages/fetch_hn_data.ts`。
  - 要修改的檔案：`package.json`。
  - _(提示：請參閱 `docs/project-structure.md` [820] 以了解階段執行器位置)。_
- **關鍵技術：**
  - TypeScript [846]、Node.js 22.x [851]、`ts-node`（通過 `npm run` 腳本）。
  - 使用 `logger`（故事 1.4）、`config`（故事 1.2）、日期工具（故事 1.4）、目錄創建邏輯（故事 1.4）、`algoliaHNClient`（故事 2.1）、`fs`/`path`（故事 2.3）。
  - _(提示：請參閱 `docs/tech-stack.md` [839-905])._
- **API 互動 / SDK 使用：**
  - 呼叫內部 `algoliaHNClient` 函數。
  - 使用 `fs.writeFileSync`。
- **數據結構：**
  - 使用 `Story`、`Comment` 類型。
  - 生成 `{storyId}_data.json` 檔案 [538-540]。
  - _(提示：請參閱 `docs/data-models.md`)._
- **環境變數：**
  - 通過 `config.ts` 讀取 `MAX_COMMENTS_PER_STORY` 和 `OUTPUT_DIR_PATH`。
  - _(提示：請參閱 `docs/environment-vars.md` [548-638])._
- **編碼標準說明：**
  - 清晰地構建腳本（設置、獲取、持久化）。
  - 使用 `async/await`。
  - 廣泛使用日誌記錄器以指示進度。 [205]
  - 考慮將主要邏輯包裝在 `async` IIFE（立即調用的函數表達式）或主要函數調用中。

## 任務 / 子任務

- [ ] 創建 `src/stages/fetch_hn_data.ts`。
- [ ] 導入日誌記錄器、配置、日期工具、`algoliaHNClient`、`fs`、`path`。
- [ ] 實現設置邏輯：初始化日誌記錄器、加載配置、獲取輸出目錄路徑、確保目錄存在。
- [ ] 實現主要獲取邏輯：
  - [ ] 呼叫 `WorkspaceTopStories`。
  - [ ] 從配置中獲取 `MAX_COMMENTS_PER_STORY`。
  - [ ] 循環處理故事：
    - [ ] 呼叫 `WorkspaceCommentsForStory`。
    - [ ] 將評論添加到故事物件。
    - [ ] 添加 `WorkspaceedAt` 時間戳。
    - [ ] 準備數據對象以進行保存。
    - [ ] 為 `{storyId}_data.json` 構建完整檔案路徑。
    - [ ] 在 `try...catch` 中使用 `fs.writeFileSync` 進行序列化和寫入檔案。
    - [ ] 記錄進度/成功/錯誤。
- [ ] 將腳本 `"stage:fetch": "ts-node src/stages/fetch_hn_data.ts"` 添加到 `package.json`。

## 測試需求

**指導：** 使用以下測試驗證實現是否符合驗收標準。

- **單元測試：** 針對底層組件（日誌記錄器、配置、客戶端、工具）的單元測試應該已經在先前的故事中存在。對於階段腳本本身的單元測試，除了檢查基本設置調用之外，可能價值有限，如果核心邏輯只是協調已測試的組件。 [915]
- **整合測試：** 不適用於該腳本，因為它*本身就是*整合測試。 [921]
- **手動/CLI 驗證（此故事的主要測試方法）：** [912, 927]
  - 執行 `npm run stage:fetch`。 [209]
  - 驗證成功執行而無錯誤。
  - 檢查日誌以獲取特定於獲取和持久化的消息 [211]。
  - 檢查帶有日期戳的輸出目錄的內容，並驗證生成的 `{storyId}_data.json` 檔案的內容是否與預期匹配（類似於故事 2.3 的驗證） [210]。
  - 修改 `package.json` 以添加該腳本（AC2）。
- _(提示：請參閱 `docs/testing-strategy.md` [907-950]，該文件將階段執行器確定為驗收測試的關鍵部分）。_

## 故事總結（代理在執行後填寫）

- **使用的代理模型：** `<Agent Model Name/Version>`
- **完成說明：** {階段執行器腳本創建並成功測試。package.json 更新。}
- **變更記錄：**
  - 初始草稿
```

---

## **End of Report for Epic 2**
