# Epic 4: LLM 摘要生成與持久化

**目標：** 與已設定的本地 Ollama 實例整合，為成功抓取的文章文本和獲取的評論生成摘要，並將這些摘要本地保存。實現一個摘要階段測試工具。

## 故事列表

### 故事 4.1: 實現 Ollama 客戶端模組

- **使用者故事 / 目標：** 作為一名開發者，我希望有一個客戶端模組可以通過 HTTP 與已設定的 Ollama API 端點互動，處理文本生成的請求和回應，以便能夠以程式化方式生成摘要。
- **詳細需求：**
  - **前置條件：** 確保已安裝並執行本地 Ollama 實例，可通過 `.env` 中定義的 URL (`OLLAMA_ENDPOINT_URL`) 訪問，並且 `.env` 中指定的模型 (`OLLAMA_MODEL`) 已下載（例如，通過 `ollama pull model_name`）。此設定的指導應記錄在專案的 README 中。
  - 建立一個新模組：`src/clients/ollamaClient.ts`。
  - 實現一個非同步函式 `generateSummary(promptTemplate: string, content: string): Promise<string | null>`。（_注意：參數名稱為清晰起見已更改_）
  - 新增配置變數 `OLLAMA_ENDPOINT_URL`（例如，`http://localhost:11434`）和 `OLLAMA_MODEL`（例如，`llama3`）到 `.env.example`。確保它們通過配置模組（`src/utils/config.ts`）加載。更新本地 `.env` 實際值。可選的 `OLLAMA_TIMEOUT_MS` 也應新增到 `.env.example`，默認值如 `120000`。
  - 在 `generateSummary` 中：
    - 使用 `promptTemplate` 和提供的 `content` 構建完整的提示字串（例如，替換模板中的占位符 `{Content Placeholder}`，或如果模板較為簡單則直接串接）。
    - 構建 Ollama API 請求的有效負載（JSON）：`{ model: configured_model, prompt: full_prompt, stream: false }`。參考 Ollama `/api/generate` 文件和 `docs/data-models.md`。
    - 使用原生 `Workspace` 發送 POST 請求到設定的 Ollama 端點 + `/api/generate`。設置適當的標頭（`Content-Type: application/json`）。使用配置的 `OLLAMA_TIMEOUT_MS` 或合理的默認值（例如，2 分鐘）。
    - 使用 `try...catch` 處理 `Workspace` 錯誤（網絡、超時）。
    - 檢查 `response.ok`。如果不 OK，記錄狀態/錯誤並返回 `null`。
    - 解析來自 Ollama 的 JSON 回應。提取生成的文本（通常在 `response` 欄位中）。參考 `docs/data-models.md`。
    - 檢查 Ollama 回應結構本身可能的錯誤（例如，`error` 欄位）。
    - 成功時返回提取的摘要字串。任何失敗時返回 `null`。
    - 記錄關鍵事件：請求啟動（提及模型）、接收回應、成功、失敗原因，可能還包括請求/回應時間，使用記錄器。
  - 在 `src/types/ollama.ts` 中定義必要的 TypeScript 類型，用於 Ollama 請求有效負載和預期回應結構（參考 `docs/data-models.md`）。
- **驗收標準（ACs）：**
  - AC1：`ollamaClient.ts` 模組存在並導出 `generateSummary`。
  - AC2：`OLLAMA_ENDPOINT_URL` 和 `OLLAMA_MODEL` 定義在 `.env.example` 中，通過配置加載，並由客戶端使用。可選的 `OLLAMA_TIMEOUT_MS` 被處理。
  - AC3：`generateSummary` 發送格式正確的 POST 請求（模型、基於模板和內容的完整提示、stream:false）到設定的 Ollama 端點/路徑，使用原生 `Workspace`。
  - AC4：網絡錯誤、超時和非 OK 的 API 回應被妥善處理、記錄，並導致返回 `null`（假設前置條件 Ollama 服務正在執行）。
  - AC5：成功的 Ollama 回應被正確解析，生成的文本被提取並作為字串返回。
  - AC6：意外的 Ollama 回應格式或內部錯誤（例如，`{"error": "..."}`）被處理、記錄，並導致返回 `null`。
  - AC7：日誌提供對客戶端與 Ollama API 互動的可見性。

---

### 故事 4.2: 定義摘要提示

- **使用者故事 / 目標：** 作為一名開發者，我希望為生成文章摘要和 HN 討論摘要定義標準化的基礎提示，並集中記錄，確保發送給 LLM 的指令一致。
- **詳細需求：**
  - 定義兩個標準化的基礎提示（`ARTICLE_SUMMARY_PROMPT`，`DISCUSSION_SUMMARY_PROMPT`），並**記錄在 `docs/prompts.md` 中**。
  - 確保這些提示在應用程式碼中可訪問，例如，通過在 `src/utils/prompts.ts` 中定義為導出的常數，該模組從 `docs/prompts.md` 中讀取或鏡像內容。
- **驗收標準（ACs）：**
  - AC1：`ARTICLE_SUMMARY_PROMPT` 文本在 `docs/prompts.md` 中定義，並包含適當的指令內容。
  - AC2：`DISCUSSION_SUMMARY_PROMPT` 文本在 `docs/prompts.md` 中定義，並包含適當的指令內容。
  - AC3：`docs/prompts.md` 中記錄的提示文本在應用程式碼中作為常數或變數可用（例如，通過 `src/utils/prompts.ts`），以供 Ollama 客戶端整合使用。

---

### 故事 4.3: 將摘要生成整合到主工作流程

- **使用者故事 / 目標：** 作為一名開發者，我希望將 Ollama 客戶端整合到主工作流程中，為每個故事的抓取文章文本（如果可用）和獲取的評論生成摘要，使用集中定義的提示並處理潛在的評論長度限制。
- **詳細需求：**
  - 修改 `src/index.ts` 或 `src/core/pipeline.ts` 中的主執行流程。
  - 匯入 `ollamaClient.generateSummary` 和提示常數/變數（例如，從 `src/utils/prompts.ts`，該模組反映 `docs/prompts.md`）。
  - 從 `.env` 中通過配置工具加載可選的 `MAX_COMMENT_CHARS_FOR_SUMMARY` 配置值。
  - 在主迴圈中迭代故事（在 Epic 3 的文章抓取/持久化之後）：
  - **文章摘要生成：**
    - 檢查 `story` 物件是否有非空的 `articleContent`。
    - 如果有：記錄 "嘗試為故事 {storyId} 生成文章摘要"，呼叫 `await generateSummary(ARTICLE_SUMMARY_PROMPT, story.articleContent)`，將結果（字串或 null）存儲為 `story.articleSummary`，記錄成功/失敗。
    - 如果沒有：設置 `story.articleSummary = null`，記錄 "跳過文章摘要生成：無內容"。
  - **討論摘要生成：**
    - 檢查 `story` 物件是否有非空的 `comments` 陣列。
    - 如果有：
      - 將 `story.comments` 陣列格式化為適合 LLM 提示的單一文本塊（例如，使用分隔符如 `---` 串接 `comment.text`）。
      - **檢查截斷限制：** 如果 `MAX_COMMENT_CHARS_FOR_SUMMARY` 被配置為正數，且 `formattedCommentsText` 長度超過該限制，則將 `formattedCommentsText` 截斷至限制，並記錄警告："評論文本已截斷至 {limit} 字元以進行摘要生成，故事 {storyId}"。
      - 記錄 "嘗試為故事 {storyId} 生成討論摘要"。
      - 呼叫 `await generateSummary(DISCUSSION_SUMMARY_PROMPT, formattedCommentsText)`。（_傳遞可能被截斷的文本_）
      - 將結果（字串或 null）存儲為 `story.discussionSummary`。記錄成功/失敗。
    - 如果沒有：設置 `story.discussionSummary = null`，記錄 "跳過討論摘要生成：無評論"。
- **驗收標準（ACs）：**
  - AC1：執行 `npm run dev` 時，執行從 Epic 1-3 的步驟，然後嘗試使用 Ollama 客戶端進行摘要生成。
  - AC2：僅當故事有 `articleContent` 時才嘗試生成文章摘要。
  - AC3：僅當故事有 `comments` 時才嘗試生成討論摘要。
  - AC4：`generateSummary` 使用正確的提示（與 `docs/prompts.md` 一致地來源）和相應的內容（文章文本或格式化/可能被截斷的評論）進行呼叫。
  - AC5：如果設置了 `MAX_COMMENT_CHARS_FOR_SUMMARY`，且評論文本超過該限制，則傳遞給 `generateSummary` 的文本被截斷，並記錄警告。
  - AC6：日誌清楚地指示每個故事的文章和討論摘要嘗試的開始、成功或失敗（包括來自客戶端的 null 返回）。
  - AC7：記憶體中的故事物件現在包含 `articleSummary`（字串/null）和 `discussionSummary`（字串/null）屬性。

---

### 故事 4.4: 本地持久化生成的摘要

_(根據最近的決定，此故事不需要更改)_

- **使用者故事 / 目標：** 作為一名開發者，我希望將生成的文章和討論摘要（或 null 占位符）保存到每個故事的本地 JSON 文件中，以便在電子郵件組裝階段使用。
- **詳細需求：**
  - 定義摘要輸出文件的結構：`{storyId}_summary.json`。內容範例：`{ "storyId": "...", "articleSummary": "...", "discussionSummary": "...", "summarizedAt": "ISO_TIMESTAMP" }`。注意，`articleSummary` 和 `discussionSummary` 可以是 `null`。
  - 在 `src/index.ts` 或 `src/core/pipeline.ts` 中匯入 `fs` 和 `path`（如果需要）。
  - 在主工作流程迴圈中，當一個故事的*兩次*摘要嘗試（文章和討論）完成後：
    - 建立一個摘要結果物件，包含 `storyId`、`articleSummary`（字串或 null）、`discussionSummary`（字串或 null）以及當前的 ISO 時間戳（`new Date().toISOString()`）。將此時間戳也新增到記憶體中的 `story` 物件中（`story.summarizedAt`）。
    - 獲取日期標記的輸出目錄的完整路徑。
    - 構建文件名：`{storyId}_summary.json`。
    - 使用 `path.join()` 構建完整的文件路徑。
    - 將摘要結果物件序列化為 JSON（`JSON.stringify(..., null, 2)`）。
    - 使用 `fs.writeFileSync` 保存 JSON 到文件，並包裹在 `try...catch` 中。
    - 記錄摘要文件保存成功或任何文件寫入錯誤。
- **驗收標準（ACs）：**
  - AC1：執行 `npm run dev` 後，日期標記的輸出目錄包含 10 個名為 `{storyId}_summary.json` 的文件。
  - AC2：每個 `_summary.json` 文件包含符合定義結構的有效 JSON。
  - AC3：`articleSummary` 欄位包含成功生成的摘要字串，否則為 `null`。
  - AC4：`discussionSummary` 欄位包含成功生成的摘要字串，否則為 `null`。
  - AC5：`summarizedAt` 欄位中存在有效的 ISO 時間戳。
  - AC6：日誌確認每個摘要文件的成功寫入或報告文件系統錯誤。

---

### 故事 4.5: 為摘要實現階段測試工具

_(需要更改以反映提示來源和可選的截斷)_

- **使用者故事 / 目標：** 作為一名開發者，我希望有一個單獨的腳本/命令來測試 LLM 摘要邏輯，使用本地持久化的數據（HN 評論、抓取的文章文本），允許獨立測試提示和 Ollama 互動。
- **詳細需求：**
  - 建立一個新的獨立腳本文件：`src/stages/summarize_content.ts`。
  - 匯入必要的模組：`fs`、`path`、`logger`、`config`、`ollamaClient`、提示常數（例如，從 `src/utils/prompts.ts`）。
  - 腳本應該：
    - 初始化記錄器，載入配置（Ollama 端點/模型、輸出目錄、**可選的 `MAX_COMMENT_CHARS_FOR_SUMMARY`**）。
    - 確定目標日期標記的目錄路徑。
    - 找到目錄中所有 `{storyId}_data.json` 文件。
    - 對於找到的每個 `storyId`：
      - 讀取 `{storyId}_data.json` 以獲取評論。將它們格式化為單一文本塊。
      - _嘗試_ 讀取 `{storyId}_article.txt`。妥善處理文件未找到的情況。存儲內容或 null。
      - 使用 `ARTICLE_SUMMARY_PROMPT` 呼叫 `ollamaClient.generateSummary` 為文章文本生成摘要（如果非 null）。
      - **應用截斷邏輯：** 如果存在評論，檢查 `MAX_COMMENT_CHARS_FOR_SUMMARY` 並在需要時截斷格式化的評論文本塊，記錄警告。
      - 使用 `DISCUSSION_SUMMARY_PROMPT` 呼叫 `ollamaClient.generateSummary` 為格式化的評論生成摘要（如果存在評論）（_傳遞可能被截斷的文本_）。
      - 構建摘要結果物件（包含摘要或 null，以及時間戳）。
      - 保存結果物件到 `{storyId}_summary.json`，存儲在相同目錄中（使用故事 4.4 的邏輯），如果存在則覆蓋。
      - 記錄每個故事 ID 的進度（讀取文件、呼叫 Ollama、截斷警告、保存結果）。
  - 在 `package.json` 中新增腳本：`"stage:summarize": "ts-node src/stages/summarize_content.ts"`。
- **驗收標準（ACs）：**
  - AC1：文件 `src/stages/summarize_content.ts` 存在。
  - AC2：腳本 `stage:summarize` 定義在 `package.json` 中。
  - AC3：執行 `npm run stage:summarize`（在 `stage:fetch` 和 `stage:scrape` 執行後）從目標目錄中讀取 `_data.json` 並嘗試讀取 `_article.txt` 文件。
  - AC4：腳本呼叫 `ollamaClient`，使用正確的提示（與 `docs/prompts.md` 一致地來源）和僅來自本地文件的內容（需要根據故事 4.1 的前置條件執行 Ollama 服務）。
  - AC5：如果設置了 `MAX_COMMENT_CHARS_FOR_SUMMARY` 並適用，評論文本在呼叫客戶端之前被截斷，並記錄警告。
  - AC6：腳本在目標目錄中建立/更新 `{storyId}_summary.json` 文件，反映 Ollama 呼叫的結果（摘要或 null）。
  - AC7：日誌顯示腳本處理每個本地找到的故事 ID，與 Ollama 互動並保存結果。
  - AC8：腳本不呼叫 Algolia API 或文章抓取模組。

## Change Log

| Change                    | Date       | Version | Description                         | Author      |
| ------------------------- | ---------- | ------- | ----------------------------------- | ----------- |
| Integrate prompts.md refs | 2025-05-04 | 0.3     | Updated stories 4.2, 4.3, 4.5       | 3-Architect |
| Added Ollama Prereq Note  | 2025-05-04 | 0.2     | Added note about local Ollama setup | 2-pm        |
| Initial Draft             | 2025-05-04 | 0.1     | First draft of Epic 4               | 2-pm        |
