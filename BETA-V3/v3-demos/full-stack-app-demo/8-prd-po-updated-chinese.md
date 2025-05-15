# BMad DiCaster 產品需求文件 (PRD)

## 目標、目的與背景

**目標：** 開發一個 Web 應用程式，提供每日精簡的 Hacker News (HN) 熱門文章摘要，以電子報形式傳送，並可透過 Web 介面存取。

**目的：** 透過整理熱門 story、提供 AI 摘要，以及可選的 AI 生成 Podcast 版本，簡化 HN 內容的閱覽流程。

**背景：** 忙碌的專業人士和愛好者希望隨時了解 HN 的最新動態，但缺乏時間篩選大量文章和討論。此應用程式將透過自動化傳送摘要內容來解決此問題。

## 功能需求 (MVP)

- **HN 內容擷取與儲存：**
  - 每日使用 HN Algolia API 擷取 Hacker News 前 30 篇熱門文章及相關留言。
  - 每日抓取並儲存最多 10 篇連結文章。
  - 儲存所有擷取到的資料（文章、留言、報導），並與日期建立關聯。
- **AI 摘要功能：**
  - 對選定的 10 篇文章進行 AI 摘要（2 段式摘要）。
  - 對選定的 10 篇文章的留言進行 AI 摘要（2 段式摘要，突顯有趣的互動）。
  - 透過環境變數設定使用本地或遠端 LLM。
- **電子報產生與傳送：**
  - 產生每日 HTML 格式電子報，包含摘要、HN 文章與報導連結，以及原始文章日期/時間。
  - 自動將電子報傳送至 Supabase 中手動設定的訂閱者清單。電子郵件清單將手動填入資料庫。Nodemailer 服務的帳戶資訊將透過環境變數提供。
- **Podcast 產生與整合：**
  - 整合 Play.ht 的 PlayNote API，從電子報內容產生 AI Podcast。
  - Webhook 處理程式，用以更新電子報中的 Podcast 連結。
- **Web 介面 (MVP)：**
  - 顯示目前及過去的電子報。
  - 在網頁內閱讀電子報內容的功能。
  - 電子報下載選項。
  - 用於收聽所產生 Podcast 的 Web 播放器。
  - 基本的行動裝置響應式設計，用於顯示電子報和 Podcast。
- **API 與觸發：**
  - 安全的 API 端點，可手動觸發每日工作流程，並使用 API 金鑰保護。
  - CLI 指令，可在本地手動觸發每日工作流程。

## 非功能性需求 (MVP)

- **效能：**
  - 系統應在合理的時間範圍內（例如 30 分鐘內）擷取 HN 文章並產生電子報，以確保及時傳送。
  - Web 介面應快速載入（例如 2 秒內），以提供流暢的使用者體驗。
- **可擴展性：**
  - 此系統設計為 MVP 初期向 3-5 位電子郵件訂閱者傳送。MVP 之後才會考慮超出此範圍的可擴展性。
- **安全性：**
  - 用於觸發每日工作流程的 API 端點必須安全，並使用 API 金鑰。
  - 使用者資料（電子郵件地址）應安全儲存。MVP 不需要其他安全性措施。
- **可靠性：**
  - MVP 未定義特定的正常運作時間或可用性需求。
  - 電子報產生與傳送流程應穩健，並能妥善處理潛在錯誤。
  - 系統必須能從本地開發環境執行。
- **可維護性：**
  - 程式碼庫應遵循良好的編碼標準，包括關注點分離。
  - 系統應採用 Facade 和 Factory 模式，以利未來擴充。
  - 系統應建構成事件驅動的管線，利用 Supabase 在每個階段擷取資料，並非同步觸發後續函式。此方法旨在減輕 Vercel 託管可能發生的逾時問題。

## 使用者互動與設計目標

本節旨在闡述使用者體驗 (UX) 的高階願景與目標，以引導設計架構師。

- **整體願景與體驗：**
  - 期望的外觀與風格是現代簡約，帶有 synthwave 技術感的發光紫色氛圍。
  - 使用者在存取和閱覽電子報內容及 Podcast 時，應享有簡潔高效的體驗。
- **關鍵互動模式：**
  - 互動模式將由設計架構師決定。
- **核心畫面/視圖 (概念性)：**
  - MVP 將包含兩個頁面：
    - 一個列表頁面，用於顯示目前及過去的電子報。
    - 一個詳細資訊頁面，用於顯示所選電子報的內容，包括：
      - 電子報下載選項。
      - 用於收聽所產生 Podcast 的 Web 播放器。
      - 排版供檢視的文章。
- **無障礙性期望：**
  - Web 介面 (Epic 6) 將遵循 `frontend-architecture.md` 中詳述的 WCAG 2.1 Level A 指南。(於 checklist 審查期間更新)
- **品牌考量 (高階)：**
  - 將提供應用程式的標誌。
  - 應用程式將使用名稱「BMad DiCaster」。
- **目標裝置/平台：**
  - 應用程式將設計為行動優先的響應式 Web 應用程式，確保在行動裝置和桌上型電腦裝置上均能良好顯示。

## 技術假設

本節旨在記錄任何現有的技術資訊，以引導架構師進行技術設計。

- 應用程式將使用 Next.js/Supabase 範本開發，並完全託管於 Vercel。
- 這意味著採用 monorepo 結構，因為前端 (Next.js) 和後端 (Supabase functions) 將位於同一個儲存庫中。
- 後端將主要利用 Vercel 和 Supabase 提供的 serverless functions。
- 前端開發將使用 Next.js 搭配 React。
- 資料儲存將由 Supabase 的 PostgreSQL 資料庫處理。
- 開發和生產環境將使用獨立的 Supabase 實例，以確保資料隔離和穩定性。
- 對於本地開發，開發人員可以利用 Supabase CLI 和 Vercel CLI 來模擬生產環境，主要用於測試函式和部署，但開發用的 Supabase 實例將是開發資料的主要來源。
- 測試將包括單元測試、整合測試（尤其是與 Supabase 的互動），以及端對端測試。
- 系統應建構成事件驅動的管線，利用 Supabase 在每個階段擷取資料，並非同步觸發後續函式，以減輕 Vercel 可能發生的逾時問題。

## Epic 總覽

_(注意：Epics 將依序開發。開發將從 Epic 1 開始，並僅在前一個 epic 完全完成並驗證後才進行下一個 epic。根據 BMAD 方法，每個 story 都必須是獨立的，並在開始下一個 story 之前完成。)_

_(注意：所有 epics 中的所有 UI 開發都必須遵循行動裝置響應式設計和 Tailwind CSS/佈景主題原則，以確保一致且可維護的使用者體驗。)_

**(所有 Epics 的服務實作通用注意事項)：** 作為任何 epic 的一部分開發的所有後端服務 (Supabase Functions) 都必須實作穩健的錯誤處理。它們應使用 Pino 進行廣泛的日誌記錄，確保所有日誌條目都包含相關的 `workflow_run_id` 以便追蹤。此外，服務必須與 `WorkflowTrackerService` 互動，以便在成功完成其任務以及發生任何故障時適當更新 `workflow_runs` 表格，並記錄適用的狀態和錯誤訊息。

- **Epic 1：專案初始化、設定與 HN 內容擷取**
  - 目標：建立基礎專案結構，包括 Next.js 應用程式、Supabase 整合、部署管線、API/CLI 觸發器、核心工作流程編排，並實作透過 `ContentAcquisitionFacade` 擷取、處理和儲存 Hacker News 文章/留言的功能，為電子報產生提供資料。實作資料庫事件機制以觸發後續處理。定義核心設定表格、種子資料，並設定測試框架。
- **Epic 2：文章抓取**
  - 目標：實作從 HN 文章中抓取並儲存連結文章的功能，豐富可用於摘要和電子報的資料。確保此功能由資料庫事件觸發，並可透過 API/CLI 進行測試 (若保留)。實作資料庫事件機制以觸發後續處理。
- **Epic 3：AI 內容摘要**
  - 目標：透過實作和使用可設定且可測試的 `LLMFacade`，整合 AI 摘要功能，以從資料庫中儲存的提示產生文章和留言的簡潔摘要。這將豐富電子報內容，可透過 API/CLI 觸發，由資料庫事件觸發，並透過 `WorkflowTrackerService` 追蹤進度。
- **Epic 4：自動化電子報建立與分發**
  - 目標：透過實作和使用可設定的 `EmailDispatchFacade`，自動化每日電子報的產生和傳送。這包括處理 Podcast 連結的可用性，可透過 API/CLI 觸發，由 `CheckWorkflowCompletionService` 編排，並透過 `WorkflowTrackerService` 追蹤狀態。
- **Epic 5：Podcast 產生整合**
  - 目標：透過實作和使用可設定的 `AudioGenerationFacade`，與音訊產生 API (初期為 Play.ht) 整合，以建立電子報的 Podcast 版本。這包括處理 webhook 以更新電子報資料和工作流程狀態。確保此功能可透過 API/CLI 觸發，進行適當編排，並使用 `WorkflowTrackerService`。
- **Epic 6：用於初始結構和內容存取的 Web 介面**
  - 目標：根據 `frontend-architecture.md` 開發一個使用者友善、響應式且易於存取的 Web 介面，以顯示電子報並提供 Podcast 內容的存取，使其符合專案的視覺和技術指南。此 epic 中的所有 UI 開發都必須遵循「synthwave 技術感發光紫色氛圍」的美學，使用 Tailwind CSS 和 Shadcn UI，確保基本的行動裝置響應式設計，符合 WCAG 2.1 Level A 無障礙指南 (包括語義化 HTML、鍵盤導覽、替代文字、色彩對比)，並使用 `next/image` 優化圖片，詳情請參閱 `frontend-architecture.txt` 和 `ui-ux-spec.txt`。

---

**Epic 1：專案初始化、設定與 HN 內容擷取**

- 目標：建立基礎專案結構，包括 Next.js 應用程式、Supabase 整合、部署管線、API/CLI 觸發器、核心工作流程編排，並實作透過 `ContentAcquisitionFacade` 擷取、處理和儲存 Hacker News 文章/留言的功能，為電子報產生提供資料。實作資料庫事件機制以觸發後續處理。定義核心設定表格、種子資料，並設定測試框架。

- **Story 1.1：** 身為開發者，我想要設定 Next.js 專案並整合 Supabase，以便擁有一個可運作的應用程式建置基礎。
  - 驗收標準：
    - 使用 Vercel/Supabase 範本初始化 Next.js 專案。
    - Supabase 成功整合至 Next.js 專案。
    - 專案程式碼庫已在 Git 儲存庫中初始化。
    - 在儲存庫的根目錄建立一個基本的專案 `README.md`，包含專案概觀、主要文件連結 (PRD、架構)，以及必要的開發者設定/執行指令。
- **Story 1.2：** 身為開發者，我想要設定部署至 Vercel 的管線，並區分開發與生產環境，以便輕鬆部署和更新應用程式。
  - 驗收標準：
    - 專案成功連結至具有獨立環境的 Vercel 專案。
    - 為 main 分支設定自動部署至生產環境。
    - 為本地開發和 Vercel 部署設定環境變數。
- **Story 1.3：** 身為開發者，我想要實作 API 和 CLI 觸發機制，以便在開發和測試期間手動觸發工作流程。
  - 驗收標準：
    - 建立一個安全的 API 端點。
    - API 端點需要驗證 (API 金鑰)。
    - API 端點 (`/api/system/trigger-workflow`) 在 `workflow_runs` 表格中建立一個條目並傳回 `jobId`。
    - API 端點傳回適當的回應以指示成功或失敗。
    - API 端點透過 API 金鑰進行保護。
    - 建立一個 CLI 指令。
    - CLI 指令呼叫 `/api/system/trigger-workflow` 端點或直接與 `WorkflowTrackerService` 互動以啟動新的工作流程執行。
    - CLI 指令在主控台提供資訊性輸出。
    - 所有 API 請求和 CLI 指令執行均已記錄，包括時間戳記和任何相關資料。
    - 所有啟動工作流程的 API 或 CLI 互動都必須在日誌中記錄 `workflow_run_id`。
    - API 和 CLI 介面遵循行動裝置響應式設計和 Tailwind/佈景主題原則。
- **Story 1.4：** 身為系統，我想要每日使用可設定的 `ContentAcquisitionFacade` 擷取前 30 篇 Hacker News 文章及相關留言，以便資料可用於電子報產生。
  - 驗收標準：
    - 在 `supabase/functions/_shared/` 中實作 `ContentAcquisitionFacade`，以抽象化與新聞資料來源 (初期為 HN Algolia API) 的互動。
    - Facade 處理特定新聞來源的 API 驗證 (若有)、請求形成和回應解析。
    - Facade 實作針對暫時性錯誤的基本重試邏輯。
    - `ContentAcquisitionFacade` 的單元測試 (模擬對 HN Algolia API 的實際 HTTP 呼叫) 達到 >80% 的覆蓋率。
    - 系統每日透過 `ContentAcquisitionFacade` 擷取前 30 篇 Hacker News 文章。
    - 系統透過 `ContentAcquisitionFacade` 擷取前 30 篇文章的相關留言。
    - 擷取到的資料 (文章和留言) 儲存在 Supabase 資料庫中，並連結至目前的 `workflow_run_id`。
    - 此功能可透過 API 和 CLI 觸發。
    - 系統記錄擷取過程的開始和完成，包括任何錯誤。
    - 完成後，服務透過 `WorkflowTrackerService` 更新 `workflow_runs` 表格的狀態和詳細資訊 (例如，擷取的文章數量)。
    - 在資料操作之前，建立並套用 `hn_posts` 和 `hn_comments` 表格 (如 `architecture.txt` 中所定義) 的 Supabase 遷移。
- **Story 1.5：定義並實作 `workflow_runs` 表格和 `WorkflowTrackerService`**
  - 目標：實作核心工作流程編排機制 (追蹤部分)。
  - 驗收標準：
    - 根據架構文件中的定義，為 `workflow_runs` 表格建立 Supabase 遷移。
    - 在 `supabase/functions/_shared/` 中實作 `WorkflowTrackerService`，包含用於啟動、更新步驟詳細資訊、遞增計數器、標記失敗以及完成工作流程執行的方法。
    - 服務包含穩健的錯誤處理和透過 Pino 進行的日誌記錄。
    - `WorkflowTrackerService` 的單元測試達到 >80% 的覆蓋率。
- **Story 1.6：實作 `CheckWorkflowCompletionService` (Supabase Cron Function)**
  - 目標：實作核心工作流程編排機制 (進程部分)。
  - 驗收標準：
    - 建立 Supabase Function `check-workflow-completion-service`。
    - 函式查詢 `workflow_runs` 及相關表格，以判斷工作流程執行是否準備好進入下一個主要階段。
    - 函式正確更新 `workflow_runs.status` 並呼叫下一個適當的服務函式。
    - 在此處或與 `NewsletterGenerationService` 結合實作處理 Podcast 連結可用性的邏輯。
    - 此函式可設定為定期執行。
    - 使用 Pino 實作全面的日誌記錄。
    - 單元測試達到 >80% 的覆蓋率。
- **Story 1.7：實作工作流程狀態 API 端點 (`/api/system/workflow-status/{jobId}`)**
  - 目標：允許開發者/管理員檢查工作流程執行的狀態。
  - 驗收標準：
    - 在 `/api/system/workflow-status/{jobId}` 建立 Next.js API Route Handler。
    - 端點使用 API 金鑰驗證進行保護。
    - 從 `workflow_runs` 表格擷取並傳回狀態詳細資訊。
    - 處理找不到 `jobId` 的情況 (404)。
    - API 端點的單元和整合測試。
- **Story 1.8：建立並記錄 `docs/environment-vars.md` 並設定 `.env.example`**
  - 目標：確保環境變數得到妥善記錄和管理。
  - 驗收標準：
    - 建立 `docs/environment-vars.md` 檔案。
    - 建立 `.env.example` 檔案。
    - 範例中的敏感資訊已遮蔽。
    - 對於每個需要憑證的第三方服務，`docs/environment-vars.md` 包含：
      - 一段簡短的說明或連結，引導使用者通常在哪裡註冊服務並取得必要的 API 金鑰或憑證。
      - 建議使用者根據預期的 MVP 使用量，檢查服務目前的免費/低階 API 速率限制。
      - 一段說明，指出超出商業服務 (如 Play.ht、遠端 LLM 或電子郵件供應商) 免費方案限制的使用量可能會產生費用，使用者應檢閱供應商的定價。
- **Story 1.9 (新增)：實作資料庫事件/Webhook：`hn_posts` 插入至文章抓取服務**
  - 目標：確保將新的 Hacker News 文章成功插入 `hn_posts` 表格後，會自動觸發 `ArticleScrapingService`。
  - 驗收標準：
    - 在 `hn_posts` 表格上針對 INSERT 操作實作 Supabase 資料庫觸發器或 webhook 機制 (例如，使用 `pg_net` 或呼叫函式的原生觸發器)。
    - 觸發器成功呼叫 `ArticleScrapingService` (Supabase Function)。
    - 呼叫將必要的參數 (如 `hn_post_id` 和 `workflow_run_id`) 傳遞給 `ArticleScrapingService`。
    - 此機制穩健，並包含觸發器/webhook 本身的錯誤處理/日誌記錄。
    - 建立單元/整合測試以驗證觸發器正確觸發，且服務以正確的參數被呼叫。
- **Story 1.10 (新增)：定義並實作核心設定表格**
  - 目標：建立儲存核心應用程式設定 (如摘要提示、電子報範本和訂閱者清單) 所需的資料庫表格。
  - 驗收標準：
    - 建立並套用 Supabase 遷移，以定義 `architecture.txt` 中指定的 `summarization_prompts` 表格結構描述。
    - 建立並套用 Supabase 遷移，以定義 `architecture.txt` 中指定的 `newsletter_templates` 表格結構描述。
    - 建立並套用 Supabase 遷移，以定義 `architecture.txt` 中指定的 `subscribers` 表格結構描述。
    - 這些表格已準備好進行資料填入 (例如，MVP 的種子資料或手動輸入)。
- **Story 1.11 (新增)：為初始設定建立種子資料**
  - 目標：將 MVP 功能開發和測試所需的初始設定資料 (提示、範本、測試訂閱者) 填入資料庫。
  - 驗收標準：
    - 建立 `supabase/seed.sql` 檔案 (或等效且有記錄的種子資料機制)。
    - 種子資料機制至少使用一個預設文章提示和一個預設留言提示填入 `summarization_prompts` 表格。
    - 種子資料機制至少使用一個預設電子報範本 (MVP 的 HTML 格式) 填入 `newsletter_templates` 表格。
    - 種子資料機制使用 1-3 個測試電子郵件地址的小清單填入 `subscribers` 表格，以進行 MVP 傳送測試。
    - 如何將種子資料套用至本地或開發 Supabase 實例的說明已記錄 (例如，在專案 `README.md` 中)。
- **Story 1.12 (新增)：設定並配置專案測試框架**
  - 目標：確保在專案生命週期的早期安裝和配置主要的測試框架 (Jest、React Testing Library、Playwright)，以便能夠實踐測試驅動開發並遵循測試策略。
  - 驗收標準：
    - Jest 和 React Testing Library (RTL) 已安裝為專案依賴項。
    - Jest 和 RTL 已配置用於 Next.js 元件和 JavaScript/TypeScript 程式碼的單元和整合測試 (例如，已設定 `jest.config.js`，已準備好必要的 Babel/TS 轉換)。
    - 已建立一個範例單元測試 (例如，針對一個簡單的元件或公用程式函式) 並使用 Jest/RTL 設定成功執行。
    - Playwright 已安裝為專案依賴項。
    - Playwright 已配置用於端對端測試 (例如，已設定 `playwright.config.ts`，已定義瀏覽器配置)。
    - 已建立一個範例 E2E 測試 (例如，在本地開發伺服器上導覽至應用程式的首頁) 並使用 Playwright 成功執行。
    - 用於執行測試 (例如，單元測試、E2E 測試) 的腳本已新增至 `package.json`。

---

**Epic 2：文章抓取**

- 目標：實作從 HN 文章中抓取並儲存連結文章的功能，豐富可用於摘要和電子報的資料。確保此功能由資料庫事件觸發，並可透過 API/CLI 進行測試 (若保留)。實作資料庫事件機制以觸發後續處理。

- **Story 2.1：** 身為系統，我想要識別前 30 篇 (可透過環境變數設定) Hacker News 文章中的 URL，以便擷取連結文章的內容。
  - 驗收標準：
    - 系統解析前 N 篇 (可透過環境變數設定) Hacker News 文章以識別 URL。
    - 系統篩選掉任何與文章抓取無關的 URL (例如，圖片、影片等連結)。
- **Story 2.2：** 身為系統，我想要使用 Cheerio 抓取所識別文章 URL 的內容，以便在電子報中提供摘要。
  - 驗收標準：
    - 系統使用 Cheerio 從所識別的文章 URL 抓取內容。
    - 系統擷取相關內容，例如文章標題、作者、發布日期和主要內文。
    - 系統處理抓取過程中可能發生的問題，例如網站錯誤或網站結構變更，並記錄錯誤以供檢閱。
- **Story 2.3：** 身為系統，我想要將抓取到的文章內容儲存在 Supabase 資料庫中，並與對應的 Hacker News 文章和工作流程執行建立關聯，以便用於摘要和電子報產生。
  - 驗收標準：
    - 抓取到的文章內容儲存在 `scraped_articles` 表格中，並連結至 `hn_post_id` 和目前的 `workflow_run_id`。
    - 系統確保儲存的資料包含所有擷取到的資訊 (標題、作者、日期、內文)。
    - `scraping_status` 和任何 `error_message` 記錄在 `scraped_articles` 表格中。
    - 完成抓取一篇文章 (成功或失敗) 後，服務透過 `WorkflowTrackerService` 更新 `workflow_runs.details` (例如，遞增已抓取計數)。
    - 在資料操作之前，建立並套用 `scraped_articles` 表格 (如 `architecture.txt` 中所定義) 的 Supabase 遷移。
- **Story 2.4：** 身為開發者，我想要透過 API 和 CLI 觸發文章抓取流程，以便手動啟動以進行測試和偵錯。
  - _架構師註記：如果主要工作流程觸發器 (Story 1.3) 處理整個管線啟動，且個別服務測試是透過直接函式呼叫或單元/整合測試完成，則此 story 可能會變得多餘。_
  - 驗收標準：
    - API 端點可以觸發文章抓取流程。
    - CLI 指令可以在本地觸發文章抓取流程。
    - 系統記錄抓取流程的開始和完成，包括遇到的任何錯誤。
    - 所有 API 請求和 CLI 指令執行均已記錄，包括時間戳記和任何相關資料。
    - 系統妥善處理部分執行 (即，如果在 Epic 1 元件 (如 `WorkflowTrackerService`) 可用之前觸發，則記錄一則訊息並結束)。
    - 如果保留用於隔離測試，則透過此觸發器啟動的所有抓取操作都必須與有效的 `workflow_run_id` 相關聯，並透過 `WorkflowTrackerService` 相應地更新 `workflow_runs` 表格。
- **Story 2.5 (新增)：實作資料庫事件/Webhook：`scraped_articles` 成功至摘要服務**
  - 目標：確保在 `scraped_articles` 中成功抓取並儲存文章後，會自動觸發 `SummarizationService`。
  - 驗收標準：
    - 在 `scraped_articles` 表格上實作 Supabase 資料庫觸發器或 webhook 機制 (例如，在 INSERT 或 UPDATE 且 `scraping_status` 為 'success' 時)。
    - 觸發器成功呼叫 `SummarizationService` (Supabase Function)。
    - 呼叫將必要的參數 (如 `scraped_article_id` 和 `workflow_run_id`) 傳遞給 `SummarizationService`。
    - 此機制穩健，並包含觸發器/webhook 本身的錯誤處理/日誌記錄。
    - 建立單元/整合測試以驗證觸發器正確觸發，且服務以正確的參數被呼叫。

---

**Epic 3：AI 內容摘要**

- 目標：透過實作和使用可設定且可測試的 `LLMFacade`，整合 AI 摘要功能，以從資料庫中儲存的提示產生文章和留言的簡潔摘要。這將豐富電子報內容，可透過 API/CLI 觸發，由資料庫事件觸發，並透過 `WorkflowTrackerService` 追蹤進度。

- **Story 3.1：** 身為系統，我想要透過實作和使用 `LLMFacade` 來整合 AI 摘要功能，以便使用各種可設定的 LLM 供應商產生文章和留言的簡潔摘要。
  - 驗收標準：
    - 在 `supabase/functions/_shared/llm-facade.ts` 中建立 `LLMFacade` 介面和具體實作 (例如，`OllamaAdapter`、`RemoteLLMApiAdapter`)。
    - 在 facade 內部或旁邊實作一個 factory function，以根據環境變數 (例如，`LLM_PROVIDER_TYPE`、`OLLAMA_API_URL`、`REMOTE_LLM_API_KEY`、`REMOTE_LLM_API_URL`、`LLM_MODEL_NAME`) 選擇適當的 LLM adapter。
    - `LLMFacade` 處理向各自的 LLM API (依設定) 發出請求，並解析其回應以擷取摘要。
    - 在 facade 內部實作針對暫時性 API 錯誤的穩健錯誤處理和重試邏輯。
    - `LLMFacade` 及其 adapter (模擬實際 HTTP 呼叫) 的單元測試達到 >80% 的覆蓋率。
    - 系統將此 `LLMFacade` 用於所有摘要任務 (文章和留言)。
    - 此整合可透過環境變數設定，以在本地和遠端 LLM 之間切換，並指定模型名稱。
- **Story 3.2：** 身為系統，我想要從資料庫擷取摘要提示，然後透過 `LLMFacade` 使用它們來產生抓取文章的 2 段式摘要，以便使用者快速掌握主要內容，並且可以輕鬆更新提示。
  - 驗收標準：
    - 服務從 `summarization_prompts` 表格擷取適當的摘要提示。
    - 系統使用透過 `LLMFacade` 擷取的提示，為每篇抓取的文章產生 2 段式摘要。
    - 產生的摘要儲存在 `article_summaries` 表格中，並連結至 `scraped_article_id` 和目前的 `workflow_run_id`。
    - 摘要準確且擷取文章的關鍵資訊。
    - 完成每篇文章摘要任務後，服務透過 `WorkflowTrackerService` 更新 `workflow_runs.details` (例如，遞增產生的文章摘要計數)。
    - (系統註記：`CheckWorkflowCompletionService` 會監控 `article_summaries` 表格，作為判斷 `workflow_run_id` 整體摘要完成情況的一部分)。
    - 在資料操作之前，建立並套用 `article_summaries` 表格 (如 `architecture.txt` 中所定義) 的 Supabase 遷移。
- **Story 3.3：** 身為系統，我想要從資料庫擷取摘要提示，然後透過 `LLMFacade` 使用它們來產生所選 HN 文章留言的 2 段式摘要，以便使用者了解主要討論內容，並且可以輕鬆更新提示。
  - 驗收標準：
    - 服務從 `summarization_prompts` 表格擷取適當的摘要提示。
    - 系統使用透過 `LLMFacade` 擷取的提示，為每篇選定的 HN 文章的留言產生 2 段式摘要。
    - 產生的摘要儲存在 `comment_summaries` 表格中，並連結至 `hn_post_id` 和目前的 `workflow_run_id`。
    - 摘要突顯有趣的互動和討論中的關鍵點。
    - 完成每個留言摘要任務後，服務透過 `WorkflowTrackerService` 更新 `workflow_runs.details` (例如，遞增產生的留言摘要計數)。
    - (系統註記：`CheckWorkflowCompletionService` 會監控 `comment_summaries` 表格，作為判斷 `workflow_run_id` 整體摘要完成情況的一部分)。
    - 在資料操作之前，建立並套用 `comment_summaries` 表格 (如 `architecture.txt` 中所定義) 的 Supabase 遷移。
- **Story 3.4：** 身為開發者，我想要透過 API 和 CLI 觸發 AI 摘要流程，以便手動啟動以進行測試和偵錯。
  - 驗收標準：
    - API 端點可以觸發 AI 摘要流程。
    - CLI 指令可以在本地觸發 AI 摘要流程。
    - 系統記錄摘要流程的輸入和輸出，包括使用的摘要提示和任何錯誤。
    - 所有 API 請求和 CLI 指令執行均已記錄，包括時間戳記和任何相關資料。
    - 系統妥善處理部分執行 (即，如果在 Epic 2 完成之前觸發，則記錄一則訊息並結束)。
    - 透過此觸發器啟動的所有摘要操作都必須與有效的 `workflow_run_id` 相關聯，並透過 `WorkflowTrackerService` 相應地更新 `workflow_runs` 表格。

---

**Epic 4：自動化電子報建立與分發**

- 目標：透過實作和使用可設定的 `EmailDispatchFacade`，自動化每日電子報的產生和傳送。這包括處理 Podcast 連結的可用性，可透過 API/CLI 觸發，由 `CheckWorkflowCompletionService` 編排，並透過 `WorkflowTrackerService` 追蹤狀態。

- **Story 4.1：** 身為系統，我想要從資料庫擷取電子報範本，以便無需修改程式碼即可更新電子報的設計和結構。
  - 驗收標準：
    - 系統從 `newsletter_templates` 資料庫表格擷取電子報範本。
- **Story 4.2：** 身為系統，我想要使用擷取的範本產生每日 HTML 格式的電子報，以便使用者收到 Hacker News 內容的簡潔摘要。
  - 驗收標準：
    - 當 `workflow_run_id` 的所有摘要都準備就緒時，`CheckWorkflowCompletionService` 會觸發 `NewsletterGenerationService`。
    - 服務從 `newsletter_templates` 表格擷取電子報範本 (來自 Story 4.1 的輸出) 以及與 `workflow_run_id` 相關聯的摘要。
    - 系統使用從資料庫擷取的範本產生 HTML 格式的電子報。
    - 電子報包含所選文章和留言的摘要。
    - 電子報包含原始 HN 文章和報導的連結。
    - 電子報包含原始文章日期/時間。
    - 產生的電子報儲存在 `newsletters` 表格中，並連結至 `workflow_run_id`。
    - 在啟動 Podcast 產生後 (作為 Epic 5 邏輯的一部分，將由此服務或在此 story 的核心任務之後由 `CheckWorkflowCompletionService` 呼叫)，服務將 `workflow_runs.status` 更新為 'generating_podcast' (或類似的適當狀態，指示已移交給 Podcast 產生)。
    - 在資料操作之前，建立並套用 `newsletters` 表格 (如 `architecture.txt` 中所定義) 的 Supabase 遷移。
- **Story 4.3：** 身為系統，我想要透過實作和使用 `EmailDispatchFacade`，並安全地提供憑證，將產生的電子報傳送給訂閱者清單，以便使用者在其收件匣中收到每日摘要。
  - 驗收標準：
    - 在 `supabase/functions/_shared/` 中實作 `EmailDispatchFacade`，以抽象化與電子郵件傳送服務 (初期為 Nodemailer via SMTP) 的互動。
    - Facade 處理設定 (例如，來自環境變數的 SMTP 設定)、電子郵件建構 (寄件人、收件人、主旨、HTML 內容) 和傳送電子郵件。
    - Facade 包含電子郵件分派的錯誤處理，並記錄相關狀態資訊。
    - `EmailDispatchFacade` (模擬實際 Nodemailer 程式庫呼叫) 的單元測試達到 >80% 的覆蓋率。
    - 一旦 `newsletters` 表格中 `workflow_run_id` 的 Podcast 連結可用 (或 Podcast 步驟已達到設定的逾時/失敗條件)，`CheckWorkflowCompletionService` 就會觸發 `NewsletterGenerationService` (特別是其傳送部分，利用 `EmailDispatchFacade`)。
    - 系統從 Supabase 資料庫擷取訂閱者電子郵件地址清單。
    - 系統使用 `EmailDispatchFacade` 將 HTML 電子報 (有條件地包含 Podcast 連結) 傳送給所有活躍的訂閱者。
    - 電子郵件服務的憑證 (例如，SMTP 伺服器詳細資訊) 透過環境變數安全地存取，並由 facade 使用。
    - 系統記錄每個訂閱者的傳送狀態 (可能透過 facade)。
    - 系統實作 Podcast 連結包含的條件邏輯 (來自 `newsletters` 表格)，並根據 PRD 處理延遲/重試，由 `CheckWorkflowCompletionService` 協調。
    - 在傳送完成或失敗時，透過 `WorkflowTrackerService` 將 `newsletters.delivery_status` (例如，'sent'、'failed') 和 `workflow_runs.status` 更新為 'completed' 或 'failed'。
    - 初始電子郵件範本包含 Podcast URL 的預留位置。
    - 在測試期間測量並記錄典型每日電子報的端對端產生時間 (從工作流程觸發到成功啟動電子郵件分派，針對少量內容)，以確保其在合理的營運時間範圍內 (目標 < 30 分鐘)。
- **Story 4.4：** 身為開發者，我想要透過 API 和 CLI 觸發電子報產生和分發流程，以便手動啟動以進行測試和偵錯。
  - 驗收標準：
    - API 端點可以觸發電子報產生和分發流程。
    - CLI 指令可以在本地觸發電子報產生和分發流程。
    - 系統記錄流程的開始和完成，包括任何錯誤。
    - 所有 API 請求和 CLI 指令執行均已記錄，包括時間戳記和任何相關資料。
    - 系統妥善處理部分執行 (即，如果在 Epic 3 完成之前觸發，則記錄一則訊息並結束)。
    - 透過此觸發器啟動的所有電子報操作都必須與有效的 `workflow_run_id` 相關聯，並透過 `WorkflowTrackerService` 相應地更新 `workflow_runs` 表格。

---

**Epic 5：Podcast 產生整合**

- 目標：透過實作和使用可設定的 `AudioGenerationFacade`，與音訊產生 API (初期為 Play.ht) 整合，以建立電子報的 Podcast 版本。這包括處理 webhook 以更新電子報資料和工作流程狀態。確保此功能可透過 API/CLI 觸發，進行適當編排，並使用 `WorkflowTrackerService`。

- **Story 5.1：** 身為系統，我想要透過實作和使用 `AudioGenerationFacade` 來整合音訊產生 API (例如，Play.ht 的 PlayNote API)，以便產生電子報內容的 AI Podcast 版本。
  - 驗收標準：
    - 在 `supabase/functions/_shared/` 中實作 `AudioGenerationFacade`，以抽象化與音訊產生服務 (初期為 Play.ht) 的互動。
    - Facade 處理特定音訊產生服務的 API 驗證、請求形成 (例如，傳送內容進行合成、提供 webhook URL) 和回應解析。
    - Facade 可透過環境變數設定 (例如，API 金鑰、使用者 ID、服務端點、webhook URL 基礎)。
    - 在 facade 內部實作針對暫時性 API 錯誤的穩健錯誤處理和重試邏輯。
    - `AudioGenerationFacade` (模擬對 Play.ht API 的實際 HTTP 呼叫) 的單元測試達到 >80% 的覆蓋率。
    - 系統將此 `AudioGenerationFacade` 用於所有 Podcast 產生任務。
    - 此整合採用 webhook 進行來自音訊產生服務的非同步狀態更新。
    - (背景：包含此邏輯的 `PodcastGenerationService` 由 `NewsletterGenerationService` 或 `CheckWorkflowCompletionService` 針對特定的 `workflow_run_id` 和 `newsletter_id` 呼叫。)
- **Story 5.2：** 身為系統，我想要透過 `AudioGenerationFacade` 將電子報內容傳送至音訊產生服務以啟動 Podcast 建立，並接收工作 ID 或初始回應，以便追蹤 Podcast 建立流程。
  - 驗收標準：
    - 系統透過 `AudioGenerationFacade` 將電子報內容 (由給定 `workflow_run_id` 的 `newsletter_id` 識別) 傳送至設定的音訊產生服務。
    - 系統透過 facade 從服務接收工作 ID 或初始回應。
    - `podcast_playht_job_id` (或通用的 `podcast_job_id`) 和 `podcast_status` (例如，'generating'、'submitted') 儲存在 `newsletters` 表格中，並連結至 `workflow_run_id`。
- **Story 5.3：** 身為系統，我想要實作一個 webhook 處理程式以從音訊產生服務接收 Podcast URL，並更新電子報資料和工作流程狀態，以便 Podcast 連結可以包含在電子報和 Web 介面中，並且整體工作流程可以繼續進行。
  - 驗收標準：
    - 系統實作一個 webhook 處理程式 (`PlayHTWebhookHandlerAPI` 位於 `/api/webhooks/playht` 或更通用的路徑，如 `/api/webhooks/audio-generation`) 以從音訊產生服務接收 Podcast URL 和狀態。
    - Webhook 處理程式從 webhook 負載中擷取 Podcast URL 和狀態 (例如，'completed'、'failed')。
    - Webhook 處理程式使用 Podcast URL 和狀態更新 `newsletters` 表格中對應的工作。
    - `PlayHTWebhookHandlerAPI` 也透過 `WorkflowTrackerService` 更新相關 `workflow_run_id` (可能需要從 webhook 中存在的 `newsletter_id` 或工作 ID 或與服務工作相關聯的 ID 查閱) 的 `workflow_runs.details` 中的 Podcast 狀態 (例如，`podcast_status: 'completed'`)。
    - 如果音訊產生服務 (例如 Play.ht) 支援，則對傳入的 webhook 實作安全驗證 (例如共享密鑰或簽章驗證) 以確保真實性。如果供應商不支援直接驗證機制，則此特定 AC 不適用，應考慮並記錄替代措施 (例如 IP 白名單，如果適用且安全)。
- **Story 5.4：** 身為開發者，我想要透過 API 和 CLI 觸發 Podcast 產生流程，以便手動啟動以進行測試和偵錯。
  - 驗收標準：
    - API 端點可以觸發 Podcast 產生流程。
    - CLI 指令可以在本地觸發 Podcast 產生流程。
    - 系統記錄流程的開始和完成，包括任何中間步驟、來自音訊產生服務的回應以及 webhook 互動。
    - 所有 API 請求和 CLI 指令執行均已記錄，包括時間戳記和任何相關資料。
    - 系統妥善處理部分執行 (即，如果在 Epic 4 元件準備就緒之前觸發，則記錄一則訊息並結束)。
    - 透過此觸發器啟動的所有 Podcast 產生操作都必須與有效的 `workflow_run_id` 和 `newsletter_id` 相關聯，並透過 `WorkflowTrackerService` 和必要的直接表格更新相應地更新 `workflow_runs` 和 `newsletters` 表格。

---

**Epic 6：用於初始結構和內容存取的 Web 介面**

- 目標：根據 `frontend-architecture.md` 開發一個使用者友善、響應式且易於存取的 Web 介面，以顯示電子報並提供 Podcast 內容的存取，使其符合專案的視覺和技術指南。此 epic 中的所有 UI 開發都必須遵循「synthwave 技術感發光紫色氛圍」的美學，使用 Tailwind CSS 和 Shadcn UI，確保基本的行動裝置響應式設計，符合 WCAG 2.1 Level A 無障礙指南 (包括語義化 HTML、鍵盤導覽、替代文字、色彩對比)，並使用 `next/image` 優化圖片，詳情請參閱 `frontend-architecture.txt` 和 `ui-ux-spec.txt`。

- **Story 6.1：** 身為開發者，我想要建立 Web 介面的初始 Next.js App Router 結構，包括核心版面配置和路由，並以 `frontend-architecture.md` 為指南，以便擁有一個基礎的前端結構。
  - 驗收標準：
    - 初始 HTML/CSS 模型 (例如，來自 Vercel v0，若有使用) 作為視覺指南，但實作將根據 `frontend-architecture.md` 使用 Next.js 和 Shadcn UI 元件。
    - 在 `app/(web)/` 路由群組內為 `/newsletters` (列表頁面) 和 `/newsletters/[newsletterId]` (詳細資訊頁面) 設定 Next.js App Router 路由。
    - 使用 Next.js App Router 慣例和 Tailwind CSS 實作根版面配置 (`app/(web)/layout.tsx`) 和任何必要的特定功能版面配置 (例如，`app/(web)/newsletters/layout.tsx`)。
    - 實作並使用 `PageWrapper.tsx` 元件 (如 `frontend-architecture.txt` 中所定義) 以實現一致的頁面樣式 (例如，邊距、最大寬度)。
    - 基本頁面結構在開發環境中正確呈現。
- **Story 6.2：** 身為使用者，我想要在 `/newsletters` 頁面上看到目前和過去的電子報清單，以便輕鬆瀏覽可用的內容。
  - 驗收標準：
    - `app/(web)/newsletters/page.tsx` 路由顯示電子報清單。
    - 電子報項目使用 `NewsletterCard.tsx` 元件顯示。
    - 開發 `NewsletterCard.tsx` 元件 (例如，以 Shadcn UI `Card` 為基礎)，至少顯示電子報標題、目標日期以及指向其詳細資訊頁面的連結/導覽。
    - `NewsletterCard.tsx` 使用 Tailwind CSS 設計樣式以符合「synthwave」主題。
    - 電子報清單的資料 (例如，ID、標題、日期) 在 `app/(web)/newsletters/page.tsx` 上使用 Supabase 伺服器用戶端進行伺服器端擷取。
    - 電子報列表頁面在常見的裝置尺寸 (行動裝置、桌上型電腦) 上具有響應式設計。
    - 清單包含相關資訊，例如電子報標題和日期。
    - 清單已分頁或提供捲動功能以處理大量電子報。
    - 在開發測試期間對電子報列表頁面的關鍵頁面載入效能 (例如，Largest Contentful Paint) 進行基準測試 (例如，使用瀏覽器開發人員工具或 Lighthouse)，以確保其符合快速載入時間的目標 (目標 < 2 秒)。
- **Story 6.3：** 身為使用者，我想要能夠從清單中選擇一份電子報，並在 `/newsletters/[newsletterId]` 頁面的網頁內閱讀其完整內容。
  - 驗收標準：
    - 按一下 `NewsletterCard` 會導覽至對應的 `app/(web)/newsletters/[newsletterId]/page.tsx` 路由。
    - 所選電子報的完整 HTML 內容使用 Supabase 伺服器用戶端進行伺服器端擷取，並以可讀格式顯示。
    - 開發 `BackButton.tsx` 元件 (例如，以 Shadcn UI `Button` 為基礎) 並整合至電子報詳細資訊頁面，允許使用者導覽回電子報清單。
    - 電子報詳細資訊頁面內容區域在常見的裝置尺寸上具有響應式設計。
    - 在開發測試期間對電子報詳細資訊頁面的關鍵頁面載入效能 (例如，Largest Contentful Paint) 進行基準測試 (例如，使用瀏覽器開發人員工具或 Lighthouse)，以確保其符合快速載入時間的目標 (目標 < 2 秒)。
- **Story 6.4：** 身為使用者，我想要在其詳細資訊頁面選擇下載目前檢視的電子報，以便離線存取。
  - 驗收標準：
    - 開發 `DownloadButton.tsx` 元件 (例如，以 Shadcn UI `Button` 為基礎)。
    - `DownloadButton.tsx` 已整合並顯示在電子報詳細資訊頁面 (`/newsletters/[newsletterId]`)。
    - 按一下按鈕會啟動電子報內容的下載 (例如，MVP 的 HTML 格式)。
- **Story 6.5：** 身為使用者，如果 Podcast 可用，我想要在其詳細資訊頁面的 Web 介面中收聽與電子報相關聯的已產生 Podcast。
  - 驗收標準：
    - 開發一個包含標準播放控制項 (播放、暫停、搜尋列、音量控制) 的 `PodcastPlayer.tsx` React 元件。
    - 實作一個 `podcastPlayerSlice.ts` Zustand store 來管理 Podcast 播放器狀態 (例如，目前曲目 URL、播放狀態、目前時間、音量)。
    - `PodcastPlayer.tsx` 元件與 `podcastPlayerSlice.ts` Zustand store 整合以進行其狀態管理。
    - 如果所顯示電子報 (從 Supabase 擷取) 有可用的 Podcast URL，則在電子報詳細資訊頁面上顯示 `PodcastPlayer.tsx` 元件。
    - `PodcastPlayer.tsx` 可以從提供的 URL 載入並播放 Podcast 音訊。
    - `PodcastPlayer.tsx` 使用 Tailwind CSS 設計樣式以符合 synthwave 主題，並且具有響應式設計。

---

## 主要參考文件

_(本節稍後將根據先前各節內容整理成較小的文件後建立)_

## MVP 後超出範圍的想法

- 使用者驗證與管理
- 訂閱管理
- 管理員儀表板
  - 檢視和更新每日 Podcast 設定
  - 摘要提示管理
  - 用於範本修改的 UI
- 增強的電子報自訂功能
- 其他內容摘要
  - 不同摘要的設定與建立
  - 支援 Hacker News 以外的內容來源
- 進階抓取技術 (例如 Playwright)

## 變更日誌

| 變更                              | 日期       | 版本 | 說明                                                                                                                               | 作者   |
| :-------------------------------- | :--------- | :--- | :--------------------------------------------------------------------------------------------------------------------------------- | :----- |
| 初稿                              | 2025-05-13 | 0.1  | 產品需求文件初稿                                                                                                                   | 2-pm   |
| 根據架構建議和 Checklist 審查更新 | 2025-05-14 | 0.3  | 整合了來自 `arch-suggested-changes.txt`、`fea-suggested-changes.txt` 和 Master Checklist 審查的變更，包括新的 story 和 AC 的完善。 | 5-posm |
