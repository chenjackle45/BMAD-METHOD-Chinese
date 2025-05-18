# {專案名稱} 專案結構

{提供專案資料夾結構的 ASCII 或 Mermaid 圖，例如以下範例。}

```plaintext
{project-root}/
├── .github/                    # CI/CD 工作流程（例如，GitHub Actions）
│   └── workflows/
│       └── main.yml
├── .vscode/                    # VSCode 設定（可選）
│   └── settings.json
├── build/                      # 編譯輸出（若適用，通常被 git 忽略）
├── config/                     # 靜態設定檔案（若有）
├── docs/                       # 專案文件（PRD、架構等）
│   ├── index.md
│   └── ...（其他 .md 檔案）
├── infra/                      # 基礎設施即程式碼（例如，CDK、Terraform）
│   └── lib/
│   └── bin/
├── node_modules/               # 專案相依套件（被 git 忽略）
├── scripts/                    # 工具腳本（建構、部署輔助等）
├── src/                        # 應用程式原始碼
│   ├── common/                 # 共用工具、型別、常數
│   ├── components/             # 可重用的 UI 元件（若有 UI）
│   ├── features/               # 特定功能模組（替代結構）
│   │   └── feature-a/
│   ├── core/                   # 核心業務邏輯
│   ├── clients/                # 外部 API/服務客戶端
│   ├── services/               # 內部服務 / 雲端 SDK 包裝器
│   ├── pages/ / routes/        # UI 頁面或 API 路由定義
│   └── main.ts / index.ts / app.ts # 應用程式進入點
├── stories/                    # 為開發生成的故事檔案（可選）
│   └── epic1/
├── test/                       # 自動化測試
│   ├── unit/                   # 單元測試（鏡像 src 結構）
│   ├── integration/            # 整合測試
│   └── e2e/                    # 端到端測試
├── .env.example                # 範例環境變數
├── .gitignore                  # Git 忽略規則
├── package.json                # 專案清單與相依套件
├── tsconfig.json               # TypeScript 設定（若適用）
├── Dockerfile                  # Docker 建構指令（若適用）
└── README.md                   # 專案概述與設定指引
```

（根據實際專案類型調整範例樹狀結構，例如，Python 專案會有 requirements.txt 等。）

## 關鍵目錄描述

docs/: 包含所有專案規劃與參考文件。
infra/: 包含基礎設施即程式碼定義（例如，AWS CDK、Terraform）。
src/: 包含主要應用程式原始碼。
common/: 跨多個模組共用的程式碼（工具、型別、常數）。避免在此放置業務邏輯。
core/ / domain/: 核心業務邏輯、實體、用例，獨立於框架/外部服務。
clients/: 負責與外部 API 或服務通訊的模組。
services/ / adapters/ / infrastructure/: 實作細節，與資料庫、雲端 SDK、框架的互動。
routes/ / controllers/ / pages/: API 請求或 UI 視圖的進入點。
test/: 包含所有自動化測試，盡量與 src/ 結構對應。
scripts/: 用於建構、部署、資料庫遷移等的輔助腳本。

## 備註

{提及任何特定的建構輸出路徑、編譯器設定指引或其他相關結構備註。}

## 變更記錄

| 變更     | 日期       | 版本 | 描述     | 作者        |
| -------- | ---------- | ---- | -------- | ----------- |
| 初始草稿 | YYYY-MM-DD | 0.1  | 初始草稿 | {代理/人員} |
| ...      | ...        | ...  | ...      | ...         |
