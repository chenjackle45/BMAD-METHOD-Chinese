# {專案名稱} 架構文件

## 技術摘要

{提供系統架構、關鍵元件、技術選擇與採用的架構模式的簡要概述（1-2 段）。參考 PRD 中的目標。}

## 高層概述

{描述主要的架構風格（例如，單體、微服務、無伺服器、事件驅動）。以概念層級解釋主要的使用者互動或資料流。}

```mermaid
{在此插入高層系統上下文或互動圖 - 例如，使用 Mermaid graph TD 或 C4 模型上下文圖}
```

## 元件視圖

{描述系統的主要邏輯元件或服務及其職責。解釋它們如何協作。}

```mermaid
{在此插入元件圖 - 例如，使用 Mermaid graph TD 或 C4 模型容器/元件圖}
```

- 元件 A：{職責描述}
- 元件 B：{職責描述}
- {src/ 目錄（如適用）：src/ 中的應用程式碼組織為邏輯模組...（簡要描述關鍵子目錄，如 clients、core、services 等，參考 `docs/project-structure.md` 獲取完整佈局）}

## 關鍵架構決策與模式

{列出重要的架構選擇與採用的模式。}

- 模式/決策 1：{例如，資料庫選擇、佇列使用、驗證策略、API 設計風格（REST/GraphQL）} - 理由：{...}
- 模式/決策 2：{...} - 理由：{...}
- （參見 `docs/coding-standards.md` 獲取詳細的程式碼模式與錯誤處理）

## 核心工作流程/序列圖（可選）

{如果有幫助，使用序列圖說明關鍵或複雜的工作流程。}

## 基礎設施與部署概述

- 雲提供商：{例如，AWS、Azure、GCP、本地部署}
- 使用的核心服務：{列出關鍵的托管服務 - 例如，Lambda、S3、Kubernetes Engine、RDS、Kafka}
- 基礎設施即程式碼（IaC）：{使用的工具 - 例如，AWS CDK、Terraform、Pulumi、ARM 模板} - 位置：{連結至 IaC 程式碼存儲庫/目錄}
- 部署策略：{例如，CI/CD 管線、手動部署步驟、藍綠部署、金絲雀部署} - 工具：{例如，Jenkins、GitHub Actions、GitLab CI}
- 環境：{列出環境 - 例如，開發、測試、正式}
- （參見 `docs/environment-vars.md` 獲取配置詳細資訊）

## 關鍵參考文件

{連結至 docs/ 資料夾中的其他相關文件。}

- docs/prd.md
- docs/epicN.md 文件
- docs/tech-stack.md
- docs/project-structure.md
- docs/coding-standards.md
- docs/api-reference.md
- docs/data-models.md
- docs/environment-vars.md
- docs/testing-strategy.md
- docs/ui-ux-spec.md（如適用）
- ...（其他相關文件）

## 變更記錄

| 變更     | 日期       | 版本 | 描述                 | 作者        |
| -------- | ---------- | ---- | -------------------- | ----------- |
| 初始草稿 | YYYY-MM-DD | 0.1  | 根據簡報建立初始草稿 | {代理/人員} |
| ...      | ...        | ...  | ...                  | ...         |
