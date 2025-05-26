# Story Definition of Done (DoD) Checklist

## Developer Agent 操作說明：

在將 story 標記為「Review」前，請逐項檢查本清單內容。請回報每一項的狀態（例如：[x] 已完成、[ ] 未完成、[N/A] 不適用），如有需要請簡要說明。

## 檢查項目：

1.  **需求達成：**

    - [ ] Story 中所列的所有功能性需求皆已實作。
    - [ ] Story 中所定義的所有驗收標準皆已達成。

2.  **程式碼標準與專案結構：**

    - [ ] 所有新增／修改的程式碼皆嚴格遵循 `Operational Guidelines`。
    - [ ] 所有新增／修改的程式碼皆符合 `Project Structure`（檔案位置、命名等）。
    - [ ] 若 story 有引入或修改技術，則所用技術／版本需符合 `Tech Stack`。
    - [ ] 若 story 涉及 API 或資料模型變更，則需遵循 `Api Reference` 與 `Data Models`。
    - [ ] 新增／修改的程式碼已採用基本安全最佳實踐（如輸入驗證、正確錯誤處理、不得硬編碼敏感資訊）。
    - [ ] 未新增任何新的 linter 錯誤或警告。
    - [ ] 程式碼於必要處已加上註解（釐清複雜邏輯，非顯而易見之處）。

3.  **測試：**

    - [ ] 依據 story 及 `Operational Guidelines` Testing Strategy，已實作所有必要的單元測試。
    - [ ] 依據 story 及 `Operational Guidelines` Testing Strategy，已實作所有必要的整合測試（如適用）。
    - [ ] 所有測試（單元、整合、E2E，如適用）皆已通過。
    - [ ] 測試涵蓋率符合專案標準（如有定義）。

4.  **功能與驗證：**

    - [ ] 開發者已手動驗證功能（例如：本地執行應用程式、檢查 UI、測試 API endpoints）。
    - [ ] 已考慮並妥善處理邊界情境及潛在錯誤狀況。

5.  **Story 管理：**

    - [ ] Story 檔案內所有任務皆已標記為完成。
    - [ ] 開發過程中所有釐清事項或決策皆已記錄於 story 檔案或適當連結。
    - [ ] Story wrap up 區段已補充與後續 story 或整體專案相關的變更說明或資訊、主要使用的 agent model，以及所有變更的 changelog 已正確更新。

6.  **相依性、建置與設定：**

    - [ ] 專案可成功建置且無錯誤。
    - [ ] 專案 lint 檢查通過。
    - [ ] 所有新增相依套件皆已於 story 需求中預先核准，或於開發過程中經 user 明確核准（核准紀錄於 story 檔案）。
    - [ ] 若有新增相依套件，皆已記錄於適當專案檔案（如 `package.json`、`requirements.txt`），並附上理由。
    - [ ] 新增且已核准的相依套件未引入已知安全漏洞。
    - [ ] 若 story 有新增環境變數或設定，皆已妥善記錄並安全處理。

7.  **文件（如適用）：**
    - [ ] 新增公開 API 或複雜邏輯之程式碼已補充相關 inline 文件（如 JSDoc、TSDoc、Python docstrings）。
    - [ ] 若變更影響使用者，已更新使用者文件。
    - [ ] 若有重大架構變更，已更新技術文件（如 README、系統架構圖）。

## 最終確認：

- [ ] 我，Developer Agent，確認上述所有適用項目皆已完成。
