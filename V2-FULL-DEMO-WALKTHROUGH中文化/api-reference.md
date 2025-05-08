# BMad Hacker Daily Digest API 參考文件

本文件描述 BMad Hacker Daily Digest 應用程式所使用的外部 API。

## 使用的外部 API

### Algolia Hacker News (HN) 搜尋 API

- **用途：** 用來抓取 Hacker News 的熱門貼文及其留言。
- **基礎網址：** `http://hn.algolia.com/api/v1`
- **認證方式：** 公開搜尋端點不需要驗證。
- **使用的主要端點：**

  - **`GET /search`（用於熱門貼文）**

    - 說明：依據搜尋參數取得貼文。此處用於從首頁獲取熱門貼文。
    - 請求參數：
      - `tags=front_page`：必填，用來篩選首頁的熱門貼文。
      - `hitsPerPage=10`：指定要抓取幾篇貼文（可依需求調整，預設為 20）。
    - 範例請求（概念性範例，使用 Node.js Workspace 原生 API）：
      ```typescript
      // 使用 Node.js 原生 Workspace API
      const url =
        "[http://hn.algolia.com/api/v1/search?tags=front_page&hitsPerPage=10](http://hn.algolia.com/api/v1/search?tags=front_page&hitsPerPage=10)";
      const response = await fetch(url);
      const data = await response.json();
      ```
    - 成功回應格式（狀態碼：`200 OK`）：請參考 `docs/data-models.md` 中的 "Algolia HN API - Story Response Subset"，主要關注 `hits` 陣列中的貼文物件。
    - 錯誤回應格式：標準 HTTP 錯誤（例如 4xx、5xx），可能為包含錯誤訊息的 JSON。

  - **`GET /search`（用於留言）**
    - 說明：取得特定貼文 ID 所有相關留言。
    - 請求參數：
      - `tags=comment,story_{storyId}`：必填，用於篩選特定 `storyId` 的留言。將 `{storyId}` 替換為實際 ID（如 `story_12345`）。
      - `hitsPerPage={maxComments}`：指定要抓取的留言數上限（來自 `.env` 中的 `MAX_COMMENTS_PER_STORY`）。
    - 範例請求（概念性範例）：
      ```typescript
      // 使用 Node.js 原生 Workspace API
      const storyId = "..."; // HN 貼文 ID
      const maxComments = 50; // 來自設定檔
      const url = `http://hn.algolia.com/api/v1/search?tags=comment,story_${storyId}&hitsPerPage=${maxComments}`;
      const response = await fetch(url);
      const data = await response.json();
      ```
    - 成功回應格式（狀態碼：`200 OK`）：請參考 `docs/data-models.md` 中的 "Algolia HN API - Comment Response Subset"，主要關注 `hits` 陣列中的留言物件。
    - 錯誤回應格式：標準 HTTP 錯誤。

- **速率限制：** 受到 Algolia 公開 API 的速率限制（通常對 HN 搜尋來說很寬鬆，但未明確定義或保證）。若出現 429 錯誤應妥善處理。
- **官方文件連結：** [https://hn.algolia.com/api](https://hn.algolia.com/api)

### Ollama API（本地執行）

- **用途：** 使用本機執行的 LLM 對抓取的文章內容與留言進行文字摘要。
- **基礎網址：** 可透過環境變數 `OLLAMA_ENDPOINT_URL` 設定（例如 `http://localhost:11434`）。
- **認證方式：** 預設本機安裝通常不需要認證。
- **使用的主要端點：**

  - **`POST /api/generate`**
    - 說明：根據模型與提示文字生成文本。此處用於摘要用途。
    - 請求主體格式：請參考 `docs/data-models.md` 中的 `OllamaGenerateRequest`，需包含 `model`（來自 `.env` 的 `OLLAMA_MODEL`）、`prompt` 與 `stream: false`。
    - 範例請求：
      ```typescript
      // 使用 Node.js 原生 Workspace API
      const ollamaUrl =
        process.env.OLLAMA_ENDPOINT_URL || "http://localhost:11434";
      const requestBody: OllamaGenerateRequest = {
        model: process.env.OLLAMA_MODEL || "llama3",
        prompt: "Summarize this text: ...",
        stream: false,
      };
      const response = await fetch(`${ollamaUrl}/api/generate`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(requestBody),
      });
      const data: OllamaGenerateResponse | { error: string } =
        await response.json();
      ```
    - 成功回應格式（狀態碼：`200 OK`）：請參考 `docs/data-models.md` 中的 `OllamaGenerateResponse`，主要欄位為 `response`，即為生成的文字。
    - 錯誤回應格式：可能回傳非 200 的狀態碼，或狀態為 `200 OK` 但內容為 `{ "error": "錯誤訊息..." }`（例如模型無法使用時）。

- **速率限制：** 本地執行一般不設限，效能取決於本機硬體。
- **官方文件連結：** [https://github.com/ollama/ollama/blob/main/docs/api.md](https://github.com/ollama/ollama/blob/main/docs/api.md)

## 提供的內部 API

- **無：** 此應用程式為獨立的 CLI 工具，不對外提供 API。

## 使用的雲端服務 SDK

- **無：** 本應用於本地執行，HTTP 請求皆使用 Node.js 原生 `Workspace` API，不依賴雲端 SDK。

## 變更紀錄

| 變更內容      | 日期        | 版本    | 說明                             | 作者         |
| ------------- | ----------- | ------- | -------------------------------- | ------------ |
| 初始草稿      | 2025-05-04  | 0.1     | 根據 PRD／Epics／Models 擬定初稿 | 3-Architect  |
