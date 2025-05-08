# BMad Hacker Daily Digest Technology Stack

## Technology Choices

| Category              | Technology                     | Version / Details        | Description / Purpose                                                                                      | Justification (Optional)                           |
| :-------------------- | :----------------------------- | :----------------------- | :--------------------------------------------------------------------------------------------------------- | :------------------------------------------------- |
| **Languages**         | TypeScript                     | 5.x (from boilerplate)   | 應用程式邏輯的主要語言                                                                                       | Required by boilerplate , strong typing            |
| **Runtime**           | Node.js                        | 22.x                     | 伺服器端執行環境                                                                                            | Required by PRD                                    |
| **Frameworks**        | N/A                            | N/A                      | 使用純 Node.js 結構                                                                                         | Boilerplate provides structure; framework overkill |
| **Databases**         | Local Filesystem               | N/A                      | 儲存中間資料工件                                                                                            | Required by PRD ; No database needed for MVP       |
| **HTTP Client**       | Node.js `Workspace` API        | Native (Node.js >=21)    | **強制要求：** 擷取外部資源（Algolia、URLs、Ollama）。**請勿使用像 `axios` 這類的函式庫。**                     | Required by PRD                                    |
| **Configuration**     | `.env` Files                   | Native (Node.js >=20.6)  | 管理環境變數。**不需要 `dotenv` 套件。**                                                                    | Standard practice; Native support                  |
| **Logging**           | Simple Console Wrapper         | Custom (`src/logger.ts`) | 用於 MVP 的基本主控台日誌（stdout/stderr）                                                                    | 滿足 PRD「基本日誌」需求；最小依賴                      |
| **Key Libraries**     | `@extractus/article-extractor` | ~8.x                     | 基本文章文字擷取                                                                                            | 專注且簡單的 MVP 擷取函式庫                            |
|                       | `date-fns`                     | ~3.x                     | 日期格式化與操作                                                                                            | 用於日期標記目錄/時間戳的乾淨 API                       |
|                       | `nodemailer`                   | ~6.x                     | 發送電子郵件摘要                                                                                            | Required by PRD                                    |
|                       | `yargs`                        | ~17.x                    | 解析階段執行器的 CLI 參數                                                                                   | 處理像 `--dry-run` 這類的階段執行器選項                  |
| **Testing**           | Jest                           | (from boilerplate)       | 單元/整合測試框架                                                                                            | Provided by boilerplate; standard                  |
| **Linting**           | ESLint                         | (from boilerplate)       | 程式碼檢查                                                                                                  | Provided by boilerplate; ensures code quality      |
| **Formatting**        | Prettier                       | (from boilerplate)       | 程式碼格式化                                                                                                | Provided by boilerplate; ensures consistency       |
| **External Services** | Algolia HN Search API          | N/A                      | 擷取 HN 新聞與留言                                                                                          | Required by PRD                                    |
|                       | Ollama API                     | N/A (local instance)     | 產生文字摘要                                                                                                | Required by PRD                                    |

## Future Considerations (Post-MVP)

- **Logging:** 實作結構化 JSON 日誌檔案（例如使用 Winston 或 Pino），以利更好的分析與持久化。
