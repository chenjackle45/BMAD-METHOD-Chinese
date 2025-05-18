# Story {EpicNum}.{StoryNum}: {Short Title Copied from Epic File}

**Status:** Draft | In-Progress | Complete

## 目標與背景

**使用者故事：** {作為[角色]，我想要[行動]，以便[效益] - 從 Epic 檔案複製或衍生}

**背景說明：** {簡要說明本 story 如何契合 Epic 目標與整體流程。如有前一 story 結果，請提及。例如：「本 story 建立於專案初始化（Story 1.1）之上，負責定義狀態持久化所需的 S3 資源……」}

## 詳細需求

{直接從對應的 `docs/epicN.md` 檔案複製本 story 的具體需求／描述。}

## 驗收標準（ACs）

{直接從對應的 `docs/epicN.md` 檔案複製本 story 的驗收標準。}

- AC1: ...
- AC2: ...
- ACN: ...

## 技術實作背景

**指引：** 請依下列細節進行實作。Developer agent 須遵循 `docs/coding-standards.md` 之專案標準，並理解 `docs/project-structure.md` 之專案結構。下方僅列出本 story 特有細節。

- **相關檔案：**

  - 新增檔案：{如 `src/services/s3-service.ts`、`test/unit/services/s3-service.test.ts`}
  - 修改檔案：{如 `lib/hacker-news-briefing-stack.ts`、`src/common/types.ts`}

- **關鍵技術：**

  - {僅列出本 story 直接使用之技術，不含整體技術棧}
  - {若為 UI story，請註明本 story 需用到的前端函式庫／框架功能}

- **API 互動／SDK 使用：**

  - {僅列出本 story 相關 API endpoint 或服務}
  - {如：「使用 `@aws-sdk/client-s3`：`S3Client`、`GetObjectCommand`、`PutObjectCommand`」}

- **UI/UX 備註：**{僅限 UI 相關 Epic 或 Story，納入相關 mockup／流程}

- **資料結構：**

  - {僅列出本 story 涉及之資料模型／實體，不含所有模型}
  - {如：「定義／使用 `AppState` 介面：`{ processedStoryIds: string[] }`」}

- **環境變數：**

  - {僅列出本 story 需用到的環境變數}
  - {如：`S3_BUCKET_NAME`（透過 `config.ts` 讀取或傳遞給 CDK）}

- **程式碼標準備註：**

  - {僅列出本 story 例外或特別相關的模式}
  - {一般請註明「遵循 `docs/coding-standards.md` 標準」}

## 測試需求

**指引：** 請依下列測試驗證實作是否符合 AC。一般測試方式請參考 `docs/testing-strategy.md`。

- **單元測試：**{僅列出本 story 具體測試需求，不含一般測試策略}
- **整合測試：**{僅本 story 需要時列出}
- **手動／CLI 驗證：**{僅本 story 需特別驗證步驟時列出}

## 任務／子任務

{從對應的 `docs/epicN.md` 檔案複製初始任務分解，並視需要擴充或細化，確保 agent 能完成所有 AC。可依測試需求增列任務與子任務。}

- [ ] 任務 1
- [ ] 任務 2
  - [ ] 子任務 2.1
- [ ] 任務 3

## Story Wrap Up（Agent 執行後填寫）

- **Agent Model Used:** `<Agent Model Name/Version>`
- **完成備註：** {實作選擇、困難點或後續需追蹤事項}
- **變更紀錄：** {如有多次修訂，請於本 story 檔案內追蹤}
  - 初稿
  - ...
