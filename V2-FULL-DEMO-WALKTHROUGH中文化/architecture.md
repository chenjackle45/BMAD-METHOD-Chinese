# BMad Hacker Daily Digest 架構文件

## 技術摘要

BMad Hacker Daily Digest 是一個命令列介面（CLI）工具，旨在為使用者提供頂尖 Hacker News（HN）故事及其相關評論討論的簡明摘要。此工具使用 TypeScript 與 Node.js（v22）構建，完全在使用者本機運行。核心功能包含一個順序管線：從 Algolia HN Search API 擷取故事與評論資料，嘗試擷取連結文章內容，利用本地 Ollama LLM 實例生成摘要，將中間資料持久化至本地檔案系統，最後組裝並以 Nodemailer 寄送 HTML 摘要。架構強調模組化與可測試性，包含強制的獨立腳本以測試每個管線階段。專案起始於 `bmad-boilerplate` 範本。

## 高階概述

應用程式遵循簡單的順序管線架構，透過手動 CLI 指令（`npm run dev` 或 `npm start`）執行。無持久化資料庫；使用本地檔案系統儲存不同步驟間的中間資料產物（擷取資料、擷取文字、摘要），資料存放於日期標記的目錄中。所有外部 HTTP 通訊（Algolia API、文章擷取、Ollama API）皆使用 Node.js 原生 `Workspace` API。

```mermaid
graph LR
    subgraph "BMad Hacker Daily Digest (Local CLI)"
        A[index.ts / CLI 觸發] --> B(core/pipeline.ts);
        B --> C{擷取 HN 資料};
        B --> D{擷取文章};
        B --> E{摘要內容};
        B --> F{組裝並寄送摘要};
        C --> G["本地檔案系統 (_data.json)"];
        D --> H["本地檔案系統 (_article.txt)"];
        E --> I["本地檔案系統 (_summary.json)"];
        F --> G;
        F --> H;
        F --> I;
    end

    subgraph External Services
        X[Algolia HN API];
        Y[文章網站];
        Z["Ollama API (本地)"];
        W[SMTP 服務];
    end

    C --> X;
    D --> Y;
    E --> Z;
    F --> W;

    style G fill:#eee,stroke:#333,stroke-width:1px
    style H fill:#eee,stroke:#333,stroke-width:1px
    style I fill:#eee,stroke:#333,stroke-width:1px
```

## 元件視圖

應用程式程式碼（`src/`）依據定義的專案結構（`docs/project-structure.md`）組織成邏輯模組。主要元件包含：

- **`src/index.ts`**：主要入口點，處理 CLI 調用並啟動管線。
- **`src/core/pipeline.ts`**：協調主要管線階段（擷取、擷取文章、摘要、寄送）的順序執行。
- **`src/clients/`**：負責與外部 API 互動的模組。
  - `algoliaHNClient.ts`：與 Algolia HN Search API 通訊。
  - `ollamaClient.ts`：與本地 Ollama API 通訊。
- **`src/scraper/articleScraper.ts`**：處理從文章 URL 擷取並抽取文字內容。
- **`src/email/`**：管理摘要組裝、HTML 渲染及透過 Nodemailer 寄送電子郵件。
  - `contentAssembler.ts`：讀取持久化資料。
  - `templates.ts`：渲染 HTML。
  - `emailSender.ts`：寄送電子郵件。
- **`src/stages/`**：包含獨立腳本（`Workspace_hn_data.ts`、`scrape_articles.ts` 等），用於獨立測試各管線階段，必要時使用本地資料。
- **`src/utils/`**：共用工具，包含設定載入（`config.ts`）、日誌（`logger.ts`）與日期處理（`dateUtils.ts`）。
- **`src/types/`**：共用 TypeScript 介面與型別。

```mermaid
graph TD
    subgraph AppComponents ["應用程式元件 (src/)"]
        Idx(index.ts) --> Pipe(core/pipeline.ts);
        Pipe --> HNClient(clients/algoliaHNClient.ts);
        Pipe --> Scraper(scraper/articleScraper.ts);
        Pipe --> OllamaClient(clients/ollamaClient.ts);
        Pipe --> Assembler(email/contentAssembler.ts);
        Pipe --> Renderer(email/templates.ts);
        Pipe --> Sender(email/emailSender.ts);

        Pipe --> Utils(utils/*);
        Pipe --> Types(types/*);
        HNClient --> Types;
        OllamaClient --> Types;
        Assembler --> Types;
        Renderer --> Types;

        subgraph StageRunnersSubgraph ["階段執行器 (src/stages/)"]
            SFetch(fetch_hn_data.ts) --> HNClient;
            SFetch --> Utils;
            SScrape(scrape_articles.ts) --> Scraper;
            SScrape --> Utils;
            SSummarize(summarize_content.ts) --> OllamaClient;
            SSummarize --> Utils;
            SEmail(send_digest.ts) --> Assembler;
            SEmail --> Renderer;
            SEmail --> Sender;
            SEmail --> Utils;
        end
    end

    subgraph Externals ["檔案系統與外部"]
        FS["本地檔案系統 (output/)"]
        Algolia((Algolia HN API))
        Websites((文章網站))
        Ollama["Ollama API (本地)"]
        SMTP((SMTP 服務))
    end

    HNClient --> Algolia;
    Scraper --> Websites;
    OllamaClient --> Ollama;
    Sender --> SMTP;

    Pipe --> FS;
    Assembler --> FS;

    SFetch --> FS;
    SScrape --> FS;
    SSummarize --> FS;
    SEmail --> FS;

    %% 對子圖套用樣式，於區塊後使用 ID
    style StageRunnersSubgraph fill:#f9f,stroke:#333,stroke-width:1px
```

## 主要架構決策與模式

- **架構風格：** CLI 執行的簡單順序管線。
- **執行環境：** 僅限本地機器；MVP 無雲端部署與資料庫。
- **資料處理：** 中間資料持久化於日期標記的本地檔案系統目錄。
- **HTTP 客戶端：** 強制使用 Node.js v22 原生 `Workspace` API 進行所有外部 HTTP 請求。
- **模組化：** 程式碼依客戶端、擷取、電子郵件、核心邏輯、工具與型別等模組組織，促進職責分離與可測試性。
- **階段測試：** 強制獨立腳本（`src/stages/*`）允許獨立測試每個管線階段。
- **設定：** 環境變數原生從 `.env` 檔案載入；無需 `dotenv` 套件。
- **錯誤處理：** 擷取失敗時優雅處理（記錄並繼續）；其他 API/網路錯誤採基本日誌。
- **日誌：** MVP 採用簡單包裝的基本主控台日誌（`src/utils/logger.ts`）；結構化檔案日誌為後期考量。
- **主要函式庫：** `@extractus/article-extractor`、`date-fns`、`nodemailer`、`yargs`。（詳見 `docs/tech-stack.md`）

## 核心工作流程 / 序列圖（主管線）

```mermaid
sequenceDiagram
    participant CLI_User as CLI 使用者
    participant Idx as src/index.ts
    participant Pipe as core/pipeline.ts
    participant Cfg as utils/config.ts
    participant Log as utils/logger.ts
    participant HN as clients/algoliaHNClient.ts
    participant FS as 本地檔案系統 [output/]
    participant Scr as scraper/articleScraper.ts
    participant Oll as clients/ollamaClient.ts
    participant Asm as email/contentAssembler.ts
    participant Tpl as email/templates.ts
    participant Snd as email/emailSender.ts
    participant Alg as Algolia HN API
    participant Web as 文章網站
    participant Olm as Ollama API [本地]
    participant SMTP as SMTP 服務

    Note right of CLI_User: 透過 'npm run dev'/'start' 觸發

    CLI_User ->> Idx: 執行腳本
    Idx ->> Cfg: 載入 .env 設定
    Idx ->> Log: 初始化日誌
    Idx ->> Pipe: runPipeline()
    Pipe ->> Log: 記錄開始
    Pipe ->> HN: fetchTopStories()
    HN ->> Alg: 請求故事
    Alg -->> HN: 故事資料
    HN -->> Pipe: stories[]
    loop 每則故事
        Pipe ->> HN: fetchCommentsForStory(storyId, max)
        HN ->> Alg: 請求評論
        Alg -->> HN: 評論資料
        HN -->> Pipe: comments[]
        Pipe ->> FS: 寫入 {storyId}_data.json
    end
    Pipe ->> Log: 記錄 HN 擷取完成

    loop 每則有 URL 的故事
        Pipe ->> Scr: scrapeArticle(story.url)
        Scr ->> Web: 請求文章 HTML [經 Workspace]
        alt 擷取成功
            Web -->> Scr: HTML 內容
            Scr -->> Pipe: articleText: string
            Pipe ->> FS: 寫入 {storyId}_article.txt
        else 擷取失敗 / 跳過
            Web -->> Scr: 錯誤 / 非 HTML / 超時
            Scr -->> Pipe: articleText: null
            Pipe ->> Log: 記錄擷取失敗/跳過
        end
    end
    Pipe ->> Log: 記錄擷取完成

    loop 每則故事
        alt 有文章內容
            Pipe ->> Oll: generateSummary(prompt, articleText)
            Oll ->> Olm: POST /api/generate [文章]
            Olm -->> Oll: 文章摘要 / 錯誤
            Oll -->> Pipe: articleSummary: string | null
        else 無文章內容
            Pipe -->> Pipe: 設定 articleSummary = null
        end
        alt 有評論
            Pipe ->> Pipe: 格式化評論為文字區塊
            Pipe ->> Oll: generateSummary(prompt, commentsText)
            Oll ->> Olm: POST /api/generate [評論]
            Olm -->> Oll: 討論摘要 / 錯誤
            Oll -->> Pipe: discussionSummary: string | null
        else 無評論
            Pipe -->> Pipe: 設定 discussionSummary = null
        end
        Pipe ->> FS: 寫入 {storyId}_summary.json
    end
    Pipe ->> Log: 記錄摘要完成

    Pipe ->> Asm: assembleDigestData(dateDirPath)
    Asm ->> FS: 讀取 _data.json, _summary.json 檔案
    FS -->> Asm: 檔案內容
    Asm -->> Pipe: digestData[]
    alt 成功組裝摘要資料
        Pipe ->> Tpl: renderDigestHtml(digestData, date)
        Tpl -->> Pipe: htmlContent: string
        Pipe ->> Snd: sendDigestEmail(subject, htmlContent)
        Snd ->> Cfg: 載入電子郵件設定
        Snd ->> SMTP: 寄送電子郵件
        SMTP -->> Snd: 成功/失敗
        Snd -->> Pipe: success: boolean
        Pipe ->> Log: 記錄電子郵件結果
    else 組裝失敗 / 無資料
        Pipe ->> Log: 記錄跳過寄送電子郵件
    end
    Pipe ->> Log: 記錄結束
```

## 基礎架構與部署概述

- **雲端供應商：** 無。於使用者本機執行。
- **核心服務使用情況：** 無（依賴外部 Algolia API、本地 Ollama、目標網站、SMTP 供應商）。
- **基礎架構即程式碼（IaC）：** 無。
- **部署策略：** 透過 CLI 手動執行（`npm run dev` 或 `npm run start`，先執行 `npm run build`）。MVP 無 CI/CD 管線需求。
- **環境：** 單一環境：本地開發機器。

## 重要參考文件

- `docs/prd.md`
- `docs/epic1.md` ... `docs/epic5.md`
- `docs/tech-stack.md`
- `docs/project-structure.md`
- `docs/data-models.md`
- `docs/api-reference.md`
- `docs/environment-vars.md`
- `docs/coding-standards.md`
- `docs/testing-strategy.md`
- `docs/prompts.md`

## 變更紀錄

| 變更     | 日期       | 版本 | 描述              | 作者        |
| -------- | ---------- | ---- | ----------------- | ----------- |
| 初始草稿 | 2025-05-04 | 0.1  | 根據 PRD 初始草稿 | 3-Architect |
