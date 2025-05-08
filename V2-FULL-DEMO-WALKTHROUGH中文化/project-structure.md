# BMad Hacker Daily Digest 專案結構

本文件概述專案的標準目錄與檔案結構。遵循此結構可確保一致性與可維護性。

```plaintext
bmad-hacker-daily-digest/
├── .github/                    # 選用：GitHub Actions 工作流程（如使用）
│   └── workflows/
├── .vscode/                    # 選用：VSCode 編輯器設定
│   └── settings.json
├── dist/                       # 編譯後的 JavaScript 輸出（由 'npm run build' 產生，git 忽略）
├── docs/                       # 專案文件（PRD、架構、Epics 等）
│   ├── architecture.md
│   ├── tech-stack.md
│   ├── project-structure.md    # 本檔案
│   ├── data-models.md
│   ├── api-reference.md
│   ├── environment-vars.md
│   ├── coding-standards.md
│   ├── testing-strategy.md
│   ├── prd.md                  # 產品需求文件
│   ├── epic1.md .. epic5.md    # Epic 詳細內容
│   └── ...
├── node_modules/               # 專案依賴（由 npm 管理，git 忽略）
├── output/                     # 預設的資料產物目錄（git 忽略）
│   └── YYYY-MM-DD/             # 依日期建立的子目錄，用於執行紀錄
│       ├── {storyId}_data.json
│       ├── {storyId}_article.txt
│       └── {storyId}_summary.json
├── src/                        # 應用程式原始碼
│   ├── clients/                # 與外部服務互動的客戶端
│   │   ├── algoliaHNClient.ts  # Algolia HN 搜尋 API 互動邏輯 [Epic 2]
│   │   └── ollamaClient.ts     # Ollama API 互動邏輯 [Epic 4]
│   ├── core/                   # 核心應用邏輯與協調
│   │   └── pipeline.ts         # 主要管線執行流程（fetch->scrape->summarize->email）
│   ├── email/                  # 電子郵件組裝、模板與發送邏輯 [Epic 5]
│   │   ├── contentAssembler.ts # 讀取本地檔案，準備摘要資料
│   │   ├── emailSender.ts      # 透過 Nodemailer 發送郵件
│   │   └── templates.ts        # HTML 電子郵件模板渲染函數
│   ├── scraper/                # 文章爬取邏輯 [Epic 3]
│   │   └── articleScraper.ts   # 使用 article-extractor 實作爬取
│   ├── stages/                 # 獨立階段測試工具腳本 [PRD 要求]
│   │   ├── fetch_hn_data.ts    # 階段執行器，對應 Epic 2
│   │   ├── scrape_articles.ts  # 階段執行器，對應 Epic 3
│   │   ├── summarize_content.ts# 階段執行器，對應 Epic 4
│   │   └── send_digest.ts      # 階段執行器，對應 Epic 5（含 --dry-run）
│   ├── types/                  # 共用 TypeScript 介面與型別
│   │   ├── hn.ts               # 型別：Story, Comment
│   │   ├── ollama.ts           # 型別：OllamaRequest, OllamaResponse
│   │   ├── email.ts            # 型別：DigestData
│   │   └── index.ts            # 本目錄型別匯出桶（barrel）檔案
│   ├── utils/                  # 共用、低階工具函數
│   │   ├── config.ts           # 載入並驗證 .env 配置 [Epic 1]
│   │   ├── logger.ts           # 簡易控制台日誌包裝器 [Epic 1]
│   │   └── dateUtils.ts        # 日期格式化輔助（使用 date-fns）
│   └── index.ts                # 主要應用程式入口（由 npm run dev/start 呼叫）[Epic 1]
├── test/                       # 自動化測試（使用 Jest）
│   ├── unit/                   # 單元測試（結構對應 src）
│   │   ├── clients/
│   │   ├── core/
│   │   ├── email/
│   │   ├── scraper/
│   │   └── utils/
│   └── integration/            # 整合測試（例如測試管線階段互動）
├── .env.example                # 範例環境變數檔案 [Epic 1]
├── .gitignore                  # Git 忽略規則（確保 node_modules、dist、.env、output/ 被忽略）
├── package.json                # 專案清單、依賴與腳本（來自樣板）
├── package-lock.json           # 鎖定檔，確保安裝一致性
└── tsconfig.json               # TypeScript 編譯器設定（來自樣板）
```

## 主要目錄說明

- `docs/`：包含所有專案規劃、架構與參考文件。
- `output/`：執行期間產生的資料產物（故事、評論、摘要）預設存放位置，應加入 `.gitignore`。路徑可透過 `.env` 配置。
- `src/`：主要應用程式原始碼。
  - `clients/`：專門與特定外部 API（Algolia、Ollama）互動的模組。
  - `core/`：協調主要應用管線步驟。
  - `email/`：處理建立與發送最終電子郵件摘要的所有相關功能。
  - `scraper/`：包含擷取與抽取文章內容的邏輯。
  - `stages/`：保存獨立可執行的腳本，用於測試每個主要管線階段。
  - `types/`：共享的 TypeScript 介面與型別定義集中地。
  - `utils/`：可重用的工具函數（配置載入、日誌、日期格式化），不屬於特定功能領域。
  - `index.ts`：主要入口點，由 `npm run dev/start` 觸發，負責初始化並啟動核心管線。
- `test/`：包含使用 Jest 撰寫的自動化測試。單元測試結構對應 `src/`。

## 備註

- 此結構透過分離關注點（clients、scraper、email、core 邏輯、stages、utils）促進模組化。
- 清晰分隔如 `clients`、`scraper`、`email` 與 `stages` 等目錄，有助於獨立開發、測試，以及針對特定功能的 AI 代理實作任務。
- `src/stages/` 中的階段執行器腳本直接回應 PRD 要求，可獨立測試管線階段。
