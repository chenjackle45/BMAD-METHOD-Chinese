以下是潛在的增補和調整摘要：

**新的技術 Epic/Stories（或對現有 Epic 的重大增補）：**

1.  **新的技術 Story（或 Epic 1 下的子任務）：工作流程編排設定**

    - **目標：** 實作核心工作流程編排機制。
    - **Stories/Tasks：**
      - **Story：定義並實作 `workflow_runs` Table 和 `WorkflowTrackerService`**
        - 驗收標準 (Acceptance Criteria)：
          - 根據架構文件中定義的 `workflow_runs` table 建立 Supabase migration。
          - 在 `supabase/functions/_shared/` 中實作 `WorkflowTrackerService`，包含用於啟動、更新步驟詳細資訊、增加計數器、標記失敗和完成工作流程執行的方法。
          - 服務包含透過 Pino 進行的強健錯誤處理和日誌記錄。
          - `WorkflowTrackerService` 的單元測試覆蓋率達到 >80%。
      - **Story：實作 `CheckWorkflowCompletionService` (Supabase Cron Function)**
        - 驗收標準 (Acceptance Criteria)：
          - 建立 Supabase Function `check-workflow-completion-service`。
          - Function 查詢 `workflow_runs` 和相關 tables，以確定工作流程執行是否準備好進入下一個主要階段（例如，從摘要生成到電子報生成，從播客啟動到交付）。
          - Function 正確更新 `workflow_runs.status`，並透過 Supabase 資料庫 webhook trigger 或直接 HTTP 呼叫（如果偏好）調用下一個適當的服務 function（例如，`NewsletterGenerationService`）。
          - 處理播客連結可用性（在發送電子郵件前的延遲/重試/超時）的邏輯在此處或與 `NewsletterGenerationService` 結合實作。
          - Function 可透過 Vercel Cron Jobs (或 `pg_cron`) 配置為定期運行。
          - 使用 Pino 實作全面的日誌記錄。
          - 單元測試覆蓋率達到 >80%。
      - **Story：實作工作流程狀態 API Endpoint (`/api/system/workflow-status/{jobId}`)**
        - 驗收標準 (Acceptance Criteria)：
          - 在 `/api/system/workflow-status/{jobId}` 建立 Next.js API Route Handler。
          - Endpoint 透過 API Key 進行身份驗證。
          - 檢索並返回給定 `jobId` 的 `workflow_runs` table 中的狀態詳細資訊。
          - 處理找不到 `jobId` 的情況 (404)。
          - API endpoint 的單元測試和整合測試。

2.  **新的技術 Story（在 Epic 3 下：AI 驅動的內容摘要）：實作 LLM Facade 和配置**

    - **目標：** 建立一個靈活的介面，用於與不同的 LLM 提供者進行摘要互動。
    - **Story：設計並實作 `LLMFacade`**
      - 驗收標準 (Acceptance Criteria)：
        - 在 `supabase/functions/_shared/llm-facade.ts` 中建立 `LLMFacade` 介面和具體實作（例如，`OllamaAdapter`、`RemoteLLMApiAdapter`）。
        - 實作 Factory function，根據環境變數 (`LLM_PROVIDER_TYPE`、`OLLAMA_API_URL`、`REMOTE_LLM_API_KEY`、`REMOTE_LLM_API_URL`、`LLM_MODEL_NAME`) 選擇 LLM adapter。
        - Facades 處理向相應的 LLM API 發出請求並解析回應。
        - 在 facade 中實作針對暫時性 API 錯誤的錯誤處理和重試邏輯。
        - Facade 和 adapters 的單元測試（模擬實際的 HTTP 呼叫）覆蓋率達到 >80%。

3.  **新的技術 Story（或相關 Epics 下的子任務）：實作外部服務的 Facades**
    - **目標：** 封裝所有外部服務互動。
    - **Stories/Tasks：**
      - **Story：實作 `HNAlgoliaFacade`** (用於 HN Content Service)
      - **Story：實作 `PlayHTFacade`** (用於 Podcast Generation Service)
      - **Story：實作 `NodemailerFacade`** (用於 Newsletter Generation Service)
      - 驗收標準 (Acceptance Criteria)（每個 facade 的通用標準）：
        - 在 `supabase/functions/_shared/` 中建立 Facade。
        - 處理特定服務的 API 身份驗證、請求形成和回應解析。
        - 實作針對暫時性錯誤的基本重試邏輯。
        - 單元測試（模擬實際的 HTTP/library 呼叫）覆蓋率達到 >80%。

**對現有 Epics/Stories 的調整：**

- **Epic 1：專案初始化、設定和 HN 內容獲取**

  - **Story 1.3 (API 和 CLI trigger)：**
    - 修改 AC："API endpoint (`/api/system/trigger-workflow`) 在 `workflow_runs` table 中建立一個 entry 並返回 `jobId`。"
    - 新增 AC："API endpoint 透過 API key 進行保護。"
    - 新增 AC："CLI command 調用 `/api/system/trigger-workflow` endpoint 或直接與 `WorkflowTrackerService` 互動以啟動新的工作流程執行。"
    - 新增 AC："所有與 API 或 CLI 互動以啟動工作流程的操作都必須在日誌中記錄 `workflow_run_id`。"
  - **Story 1.4 (Retrieve HN Posts & Comments)：**
    - 修改 AC："檢索到的資料（posts 和 comments）儲存在 Supabase 資料庫中，並連結到當前的 `workflow_run_id`。"
    - 新增 AC："完成後，服務透過 `WorkflowTrackerService` 更新 `workflow_runs` table 的狀態和詳細資訊（例如，獲取的 posts 數量）。"
    - 新增 AC："對於每個新的 `hn_posts` 記錄，配置一個資料庫事件/webhook 以觸發 Article Scraping Service (Epic 2)，傳遞 `hn_post_id` 和 `workflow_run_id`。"

- **Epic 2：文章抓取 (Article Scraping)**

  - **Story 2.2 & 2.3 (Scrape & Store)：**
    - 修改 AC："抓取的文章內容儲存在 `scraped_articles` table 中，並連結到 `hn_post_id` 和當前的 `workflow_run_id`。"
    - 新增 AC：`scraping_status` 和任何 `error_message` 記錄在 `scraped_articles` table 中。
    - 新增 AC："完成抓取文章（成功或失敗）後，服務透過 `WorkflowTrackerService` 更新 `workflow_runs.details`（例如，增加抓取計數）。"
    - 新增 AC："對於每個成功抓取的文章，配置一個資料庫事件/webhook 以觸發該文章的 Summarization Service (Epic 3)，傳遞 `scraped_article_id` 和 `workflow_run_id`。"
  - **Story 2.4 (Trigger scraping via API/CLI)：** 如果主要工作流程 trigger (Story 1.3) 處理整個管道啟動，則此 story 可能會變得多餘。如果保留用於獨立測試，請確保它也適用於 `workflow_run_id` 概念。

- **Epic 3：AI 驅動的內容摘要**

  - **Story 3.1 (Integrate AI summarization)：** 細化為 "與 `LLMFacade` 整合以進行摘要。"
  - **Story 3.2 & 3.3 (Generate Summaries)：**
    - 修改 AC："摘要儲存在 `article_summaries` 或 `comment_summaries` tables 中，並連結到 `scraped_article_id` (用於文章) 或 `hn_post_id` (用於 comments)，以及當前的 `workflow_run_id`。"
    - 新增 AC："服務從 `summarization_prompts` table 檢索 prompts。"
    - 新增 AC："完成每個摘要任務後，服務透過 `WorkflowTrackerService` 更新 `workflow_runs.details`（例如，增加生成的摘要計數）。"
    - 新增 AC (隱含)：`CheckWorkflowCompletionService` 將監控這些 tables，以確定何時完成 `workflow_run_id` 的所有摘要。

- **Epic 4：自動化電子報建立和分發**

  - **Story 4.1 & 4.2 (Retrieve template & Generate Newsletter)：**
    - 修改 AC：當 `workflow_run_id` 的所有摘要都準備好時，`NewsletterGenerationService` 由 `CheckWorkflowCompletionService` 觸發。
    - 新增 AC："服務從 `newsletter_templates` table 檢索電子報模板（來自 Story 4.1 輸出）以及與 `workflow_run_id` 相關聯的摘要。"
    - 修改 AC："生成的電子報儲存在 `newsletters` table 中，並連結到 `workflow_run_id`。"
    - 新增 AC："服務在啟動播客生成後（作為 Epic 5 邏輯的一部分，該邏輯將由本服務或在本 story 核心任務完成後由 `CheckWorkflowCompletionService` 調用）將 `workflow_runs.status` 更新為 'generating_podcast'（或類似的適當狀態，表示已交接給播客生成）。"
  - **Story 4.3 (Send newsletter)：**
    - 修改 AC：一旦 `workflow_run_id` 的播客連結在 `newsletters` table 中可用（或已達到播客步驟的配置超時/失敗條件），`NewsletterGenerationService`（特別是其交付部分）由 `CheckWorkflowCompletionService` 觸發。
    - 修改 AC：根據 PRD 實作播客連結包含的條件邏輯，並處理延遲/重試，由 `CheckWorkflowCompletionService` 協調。
    - 新增 AC：透過 `WorkflowTrackerService` 更新 `newsletters.delivery_status`（例如，'sent'、'failed'）和 `workflow_runs.status` 為 'completed' 或 'failed'。

- **Epic 5：播客生成整合**
  - **Story 5.1, 5.2, 5.3 (Integrate Play.ht, Send Content, Webhook Handler)：**
    - 修改 AC：`PodcastGenerationService` 由 `NewsletterGenerationService` (或 `CheckWorkflowCompletionService`) 調用，用於特定的 `workflow_run_id` 和 `newsletter_id`。
    - 修改 AC：`podcast_playht_job_id` 和 `podcast_status` 儲存在 `newsletters` table 中。
    - 修改 AC：`PlayHTWebhookHandlerAPI` (`/api/webhooks/playht`) 使用播客 URL 和狀態更新 `newsletters` table。
    - 新增 AC：`PlayHTWebhookHandlerAPI` 也透過 `WorkflowTrackerService` 更新相關 `workflow_run_id` 的 `workflow_runs.details`，包含播客狀態（需要從 `newsletter_id` 或 `playht_job_id` 查找）。

**所有 Epics 的一般考量：**

- **錯誤處理和日誌記錄：** 強調所有服務必須實作強健的錯誤處理，使用 Pino 進行廣泛的日誌記錄（包括 `workflow_run_id`），並在成功或失敗時使用 `WorkflowTrackerService` 適當更新 `workflow_runs`。
- **資料庫 Webhook/Trigger 配置：** 新增與設定實際 Supabase 資料庫 webhooks（例如，`pg_net` 呼叫）或 triggers 相關的技術任務/stories，這些 webhooks/triggers 連接管道步驟（例如，`hn_posts` 插入觸發 `ArticleScrapingService`）。這是事件驅動流程的關鍵實作細節。
- **環境變數管理：** 一個用於建立和記錄 `docs/environment-vars.md` 並設定 `.env.example` 的 story。
