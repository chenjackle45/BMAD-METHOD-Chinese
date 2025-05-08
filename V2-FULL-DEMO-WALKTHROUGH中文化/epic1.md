# Epic 1: 專案初始化與核心設定

**目標：** 使用 "bmad-boilerplate" 初始化專案，管理相依套件，設定 `.env` 和配置載入，建立基本 CLI 進入點，設定基本日誌記錄與輸出目錄結構。這為後續的開發工作提供了基礎設置。

## 故事清單

### 故事 1.1: 從 Boilerplate 初始化專案

- **使用者故事 / 目標：** 作為一名開發者，我希望使用 `bmad-boilerplate` 設置初始專案結構，以便擁有標準工具（TS、Jest、ESLint、Prettier）、配置和腳本。
- **詳細需求：**
  - 複製或複製 `bmad-boilerplate` 的內容到新專案的根目錄。
  - 在專案根目錄初始化一個 git 儲存庫（如果複製時尚未完成）。
  - 確保 `.gitignore` 檔案來自 boilerplate。
  - 執行 `npm install` 下載並安裝 `package.json` 中指定的所有 `devDependencies`。
  - 驗證核心 boilerplate 腳本（`lint`、`format`、`test`、`build`）在初始程式碼庫上執行無錯誤。
- **驗收標準 (ACs)：**
  - AC1: 專案目錄包含來自 `bmad-boilerplate` 的檔案和結構。
  - AC2: 存在 `node_modules` 目錄，並包含對應於 `devDependencies` 的套件。
  - AC3: `npm run lint` 指令成功完成，且未報告任何 lint 錯誤。
  - AC4: `npm run format` 指令成功完成，可能根據 Prettier 規則進行格式化更改。再次執行應無任何變更。
  - AC5: `npm run test` 指令成功執行 Jest（此階段可能報告 "no tests found"，這是可以接受的）。
  - AC6: `npm run build` 指令成功執行，並在 `dist` 目錄中建立編譯後的 JavaScript 輸出。
  - AC7: `.gitignore` 檔案存在，並包含 `node_modules/`、`.env`、`dist/` 等條目，符合 boilerplate 的規範。

---

### 故事 1.2: 設定環境配置

- **使用者故事 / 目標：** 作為一名開發者，我希望使用 `.env` 檔案建立環境配置機制，以便在版本控制之外管理機密和設定（如輸出路徑），遵循 boilerplate 的慣例。
- **詳細需求：**
  - 驗證 `.env.example` 檔案是否存在（來自 boilerplate）。
  - 在 `.env.example` 中新增初始配置變數 `OUTPUT_DIR_PATH=./output`。
  - 通過複製 `.env.example` 建立本地 `.env` 檔案。如有需要，填入 `OUTPUT_DIR_PATH`（可保留預設值）。
  - 實作一個工具模組（例如 `src/config.ts`），在應用程式啟動時從 `.env` 檔案載入環境變數。
  - 該工具模組應匯出載入的配置值（初始僅包含 `OUTPUT_DIR_PATH`）。
  - 確保 `.env` 檔案列入 `.gitignore`，且未被提交。
- **驗收標準 (ACs)：**
  - AC1: 使用 Node.js 22 原生支援處理 `.env` 檔案，無需使用 `dotenv`。
  - AC2: `.env.example` 檔案存在，並被 git 追蹤，且包含 `OUTPUT_DIR_PATH=./output`。
  - AC3: `.env` 檔案本地存在，但未被 git 追蹤。
  - AC4: 存在一個配置模組（如 `src/config.ts`），並在應用程式啟動時成功載入 `.env` 中的 `OUTPUT_DIR_PATH` 值。
  - AC5: 應用程式程式碼中可存取載入的 `OUTPUT_DIR_PATH` 值。

---

### 故事 1.3: 實作基本 CLI 進入點與執行

- **使用者故事 / 目標：** 作為一名開發者，我希望有一個基本的 `src/index.ts` 進入點，可以通過 boilerplate 的 `dev` 和 `start` 腳本執行，為應用程式邏輯提供運行基礎。
- **詳細需求：**
  - 在 `src/index.ts` 建立主要應用程式進入點檔案。
  - 在 `src/index.ts` 中實作最小程式碼以：
    - 匯入配置載入機制（來自故事 1.2）。
    - 在控制台記錄簡單的啟動訊息（例如 "BMad Hacker Daily Digest - Starting Up..."）。
    - （可選）記錄載入的 `OUTPUT_DIR_PATH` 以驗證配置載入。
  - 使用 boilerplate 腳本確認執行。
- **驗收標準 (ACs)：**
  - AC1: `src/index.ts` 檔案存在。
  - AC2: 執行 `npm run dev` 通過 `ts-node` 執行 `src/index.ts`，並在控制台記錄啟動訊息。
  - AC3: 執行 `npm run build` 成功編譯 `src/index.ts`（及任何匯入）到 `dist` 目錄。
  - AC4: 執行 `npm start`（在成功編譯後）從 `dist` 執行編譯程式碼，並在控制台記錄啟動訊息。

---

### 故事 1.4: 設定基本日誌記錄與輸出目錄

- **使用者故事 / 目標：** 作為一名開發者，我希望有一個基本的控制台日誌記錄機制和動態建立日期標記的輸出目錄，以便應用程式提供執行回饋並準備儲存後續史詩中的數據工件。
- **詳細需求：**
  - 實作一個簡單、可重用的日誌記錄工具模組（例如 `src/logger.ts`）。初始可封裝 `console.log`、`console.warn`、`console.error`。
  - 重構 `src/index.ts` 使用此 `logger` 記錄啟動訊息。
  - 在 `src/index.ts`（或由其呼叫的設定函式）中：
    - 從配置中檢索 `OUTPUT_DIR_PATH`（載入於故事 1.2）。
    - 獲取當前日期，格式為 'YYYY-MM-DD'。
    - 構建日期標記子目錄的完整路徑（例如 `${OUTPUT_DIR_PATH}/YYYY-MM-DD`）。
    - 檢查基礎輸出目錄是否存在；若不存在，則建立。
    - 檢查日期標記子目錄是否存在；若不存在，則遞迴建立。使用 Node.js 的 `fs` 模組（例如 `fs.mkdirSync(path, { recursive: true })`）。
    - 使用 logger 記錄當前執行使用的輸出目錄完整路徑（例如 "Output directory for this run: ./output/2025-05-04"）。
- **驗收標準 (ACs)：**
  - AC1: 存在一個日誌記錄工具模組（如 `src/logger.ts`），並在 `src/index.ts` 中用於控制台輸出。
  - AC2: 執行 `npm run dev` 或 `npm start` 通過 logger 記錄啟動訊息。
  - AC3: 執行應用程式時，若基礎輸出目錄（如 `.env` 中定義的 `./output`）不存在，則建立該目錄。
  - AC4: 執行應用程式時，若基礎輸出目錄內的日期標記子目錄（如 `./output/2025-05-04`）不存在，則建立該子目錄。
  - AC5: 應用程式記錄一條訊息，指示為當前執行建立/使用的日期標記輸出目錄的完整路徑。
  - AC6: 應用程式在執行這些設置步驟後正常退出（目前為止）。

## 變更記錄

| 變更 | 日期       | 版本 | 描述          | 作者 |
| ---- | ---------- | ---- | ------------- | ---- |
| 初稿 | 2025-05-04 | 0.1  | Epic 1 的初稿 | 2-pm |
