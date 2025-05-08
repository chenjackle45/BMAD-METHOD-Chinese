# BMad Hacker Daily Digest 環境變數

## 組態載入機制

本專案的環境變數透過專案根目錄下的 `.env` 檔案管理。應用程式利用 Node.js（v20.6.0 及以上版本）內建對 `.env` 檔案的支援，這表示 **不需要額外安裝 `dotenv` 套件**。

定義在 `.env` 檔案中的變數會在 Node.js 應用程式啟動時自動載入至 `process.env`。建議在 `src/utils/config.ts` 模組中集中處理這些變數的存取與驗證。

## 必要變數

下表列出應用程式使用的環境變數。應於版本庫中維護一份 `.env.example` 檔案，並以預設值或占位符填入下列變數。

| Variable Name                   | 說明                                                                 | 範例／預設值                               | 是否必要 | 是否敏感 | 來源           |
| :------------------------------ | :------------------------------------------------------------------ | :---------------------------------------- | :-------- | :--------- | :------------- |
| `OUTPUT_DIR_PATH`               | 儲存輸出資料成品的檔案系統路徑                                       | `./output`                                | 是        | 否         | Epic 1         |
| `MAX_COMMENTS_PER_STORY`        | 每則 HN 貼文要抓取的最大留言數                                       | `50`                                      | 是        | 否         | PRD            |
| `OLLAMA_ENDPOINT_URL`           | 本地 Ollama API 實例的基礎 URL                                      | `http://localhost:11434`                  | 是        | 否         | Epic 4         |
| `OLLAMA_MODEL`                  | 用於摘要的 Ollama 模型名稱                                          | `llama3`                                  | 是        | 否         | Epic 4         |
| `EMAIL_HOST`                    | 用於發送電子郵件的 SMTP 伺服器主機名稱                              | `smtp.example.com`                        | 是        | 否         | Epic 5         |
| `EMAIL_PORT`                    | SMTP 伺服器的連接埠                                                 | `587`                                     | 是        | 否         | Epic 5         |
| `EMAIL_SECURE`                  | 是否使用 TLS/SSL（465 為 `true`，587/STARTTLS 為 `false`）          | `false`                                   | 是        | 否         | Epic 5         |
| `EMAIL_USER`                    | SMTP 驗證用戶名                                                     | `user@example.com`                        | 是        | **是**     | Epic 5         |
| `EMAIL_PASS`                    | SMTP 驗證密碼                                                       | `your_smtp_password`                      | 是        | **是**     | Epic 5         |
| `EMAIL_FROM`                    | 寄件人電子郵件地址（可能需要特定格式）                              | `"BMad Digest <digest@example.com>"`      | 是        | 否         | Epic 5         |
| `EMAIL_RECIPIENTS`              | 收件人電子郵件地址的逗號分隔清單                                   | `recipient1@example.com,r2@test.org`      | 是        | 否         | Epic 5         |
| `NODE_ENV`                      | 執行環境（影響某些函式庫行為）                                     | `development`                             | 否        | 否         | Standard Node  |
| `SCRAPE_TIMEOUT_MS`             | _選填：_ 抓取文章請求的逾時時間（毫秒）                            | `15000`（15秒）                           | 否        | 否         | Good Practice  |
| `OLLAMA_TIMEOUT_MS`             | _選填：_ Ollama API 請求的逾時時間（毫秒）                          | `120000`（2分鐘）                         | 否        | 否         | Good Practice  |
| `LOG_LEVEL`                     | _選填：_ 控制日誌詳盡程度（例如 debug、info）                       | `info`                                    | 否        | 否         | Good Practice  |
| `MAX_COMMENT_CHARS_FOR_SUMMARY` | _選填：_ 傳給 LLM 進行摘要的留言總字數上限                         | 10000 / null（未設定則使用全部）         | 否        | 否         | Arch Decision  |
| `SCRAPER_USER_AGENT`            | _選填：_ 抓取請求所使用的自訂 User-Agent 標頭                       | "BMadHackerDigest/0.1"（預設寫在程式碼中）| 否        | 否         | Arch Decision  |

## 備註

- **機密管理：** 敏感變數（如 `EMAIL_USER`, `EMAIL_PASS`）**絕不可**提交至版本控制。應於 `.gitignore` 中排除 `.env` 檔（已於樣板專案設定）。
- **`.env.example`：** 請在版本庫中維護一份 `.env.example` 檔案，內容與上述變數一致，並使用預設值或占位符，供文件與本地開發使用。
- **驗證建議：** 建議在 `src/utils/config.ts` 中實作驗證邏輯，以確保應用啟動時已載入必要變數，並可視情況驗證格式。

## 變更紀錄

| 變更內容      | 日期        | 版本    | 說明                                     | 作者         |
| ------------- | ----------- | ------- | ---------------------------------------- | ------------ |
| 初始草稿      | 2025-05-04  | 0.1     | 依據 PRD/Epics 需求擬定初稿              | 3-Architect  |
