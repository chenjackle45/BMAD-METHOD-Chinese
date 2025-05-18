# BMad Hacker Daily Digest 資料模型

本文件定義了應用程式中使用的核心資料結構、持久化資料檔案的格式，以及相關的 API 輸入輸出格式。這些類型通常會放在 `src/types/` 目錄下。

## 1. 核心應用實體 / 領域物件（記憶體中）

這些 TypeScript 介面代表在管線執行期間操作的主要資料物件。

### `Comment`

- **說明：** 代表從 Algolia API 取得的單一 Hacker News 評論。
- **Schema / 介面定義 (`src/types/hn.ts`)：**
  ```typescript
  export interface Comment {
    commentId: string; // Unique identifier (from Algolia objectID)
    commentText: string | null; // Text content of the comment (nullable from API)
    author: string | null; // Author's HN username (nullable from API)
    createdAt: string; // ISO 8601 timestamp string of comment creation
  }
  ```

### `Story`

- **說明：** 代表 Hacker News 的一則故事，最初從 Algolia 取得，並在管線執行過程中逐步加入評論、爬取內容與摘要。
- **Schema / 介面定義 (`src/types/hn.ts`)：**

  ```typescript
  import { Comment } from "./hn";

  export interface Story {
    storyId: string; // Unique identifier (from Algolia objectID)
    title: string; // Story title
    articleUrl: string | null; // URL of the linked article (can be null from API)
    hnUrl: string; // URL to the HN discussion page (constructed)
    points?: number; // HN points (optional)
    numComments?: number; // Number of comments reported by API (optional)

    // Data added during pipeline execution
    comments: Comment[]; // Fetched comments [Added in Epic 2]
    articleContent: string | null; // Scraped article text [Added in Epic 3]
    articleSummary: string | null; // Generated article summary [Added in Epic 4]
    discussionSummary: string | null; // Generated discussion summary [Added in Epic 4]
    fetchedAt: string; // ISO 8601 timestamp when story/comments were fetched [Added in Epic 2]
    summarizedAt?: string; // ISO 8601 timestamp when summaries were generated [Added in Epic 4]
  }
  ```

### `DigestData`

- **說明：** 代表組裝最終電子郵件摘要時，單一故事所需的整合資料。由讀取持久化檔案產生。
- **Schema / 介面定義 (`src/types/email.ts`)：**
  ```typescript
  export interface DigestData {
    storyId: string;
    title: string;
    hnUrl: string;
    articleUrl: string | null;
    articleSummary: string | null;
    discussionSummary: string | null;
  }
  ```

## 2. API 輸入輸出格式

這些描述外部 API 請求與回應有效部分的格式。

### Algolia HN API - 故事回應子集

- **說明：** 從 Algolia HN 搜尋 API 回應中，針對首頁故事擷取的相關欄位。
- **Schema（概念性 JSON）：**
  ```json
  {
    "hits": [
      {
        "objectID": "string", // Used as storyId
        "title": "string",
        "url": "string | null", // Used as articleUrl
        "points": "number",
        "num_comments": "number"
        // ... other fields ignored
      }
      // ... more hits (stories)
    ]
    // ... other top-level fields ignored
  }
  ```

### Algolia HN API - 評論回應子集

- **說明：** 從 Algolia HN 搜尋 API 回應中，針對故事相關評論擷取的相關欄位。
- **Schema（概念性 JSON）：**
  ```json
  {
    "hits": [
      {
        "objectID": "string", // Used as commentId
        "comment_text": "string | null",
        "author": "string | null",
        "created_at": "string" // ISO 8601 format
        // ... other fields ignored
      }
      // ... more hits (comments)
    ]
    // ... other top-level fields ignored
  }
  ```

### Ollama `/api/generate` 請求

- **說明：** 傳送至本地 Ollama 實例以產生摘要的請求負載。
- **Schema (`src/types/ollama.ts` 或內嵌)：**
  ```typescript
  export interface OllamaGenerateRequest {
    model: string; // e.g., "llama3" (from config)
    prompt: string; // The full prompt including context
    stream: false; // Required to be false for single response
    // system?: string; // Optional system prompt (if used)
    // options?: Record<string, any>; // Optional generation parameters
  }
  ```

### Ollama `/api/generate` 回應

- **說明：** 在 `stream: false` 時，從 Ollama API 預期收到的相關欄位。
- **Schema (`src/types/ollama.ts` 或內嵌)：**
  ```typescript
  export interface OllamaGenerateResponse {
    model: string;
    created_at: string; // ISO 8601 timestamp
    response: string; // The generated summary text
    done: boolean; // Should be true if stream=false and generation succeeded
    // Optional fields detailing context, timings, etc. are ignored for MVP
    // total_duration?: number;
    // load_duration?: number;
    // prompt_eval_count?: number;
    // prompt_eval_duration?: number;
    // eval_count?: number;
    // eval_duration?: number;
  }
  ```
  _(注意：錯誤回應可能具有不同結構，例如 `{ "error": "message" }`)_

## 3. 資料庫結構

- **無：** 本應用程式在 MVP 階段不使用資料庫，資料皆持久化至本地檔案系統。

## 4. 狀態檔案格式（本地檔案系統持久化）

這些描述儲存在 `output/YYYY-MM-DD/` 目錄下的檔案格式。

### `{storyId}_data.json`

- **用途：** 儲存取得的故事元資料及相關評論。
- **格式：** JSON
- **Schema 定義（符合儲存時的 `Story` 類型欄位）：**
  ```json
  {
    "storyId": "string",
    "title": "string",
    "articleUrl": "string | null",
    "hnUrl": "string",
    "points": "number | undefined",
    "numComments": "number | undefined",
    "fetchedAt": "string", // ISO 8601 timestamp
    "comments": [
      // Array of Comment objects
      {
        "commentId": "string",
        "commentText": "string | null",
        "author": "string | null",
        "createdAt": "string" // ISO 8601 timestamp
      }
      // ... more comments
    ]
  }
  ```

### `{storyId}_article.txt`

- **用途：** 儲存成功爬取的連結文章純文字內容。
- **格式：** 純文字檔（`.txt`）
- **Schema 定義：** 無（內容即為原始擷取字串）。檔案僅在爬取成功時存在。

### `{storyId}_summary.json`

- **用途：** 儲存產生的文章與討論摘要。
- **格式：** JSON
- **Schema 定義：**
  ```json
  {
    "storyId": "string",
    "articleSummary": "string | null", // Null if scraping failed or summarization failed
    "discussionSummary": "string | null", // Null if no comments or summarization failed
    "summarizedAt": "string" // ISO 8601 timestamp
  }
  ```

## 變更紀錄

| 變更          | 日期       | 版本    | 說明                          | 作者        |
| ------------- | ---------- | ------- | ---------------------------- | ----------- |
| 初稿          | 2025-05-04 | 0.1     | 根據 Epics 撰寫初稿           | 3-Architect |
