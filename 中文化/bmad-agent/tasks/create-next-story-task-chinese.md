# 建立下一個 Story 任務

## 目的

根據專案進度和 epic 定義識別下一個合乎邏輯的 story，然後使用 `Story Template` 準備一個全面、獨立且可執行的 story 檔案。此任務確保 story 充實所有必要的技術背景、需求和驗收標準，使其可供 Developer Agent 高效實作，並盡可能減少額外研究的需求。

## 此任務的輸入

- 存取專案的文件儲存庫，特別是：
  - `docs/index.md` (以下簡稱「索引文件」)
  - 所有 Epic 檔案 (例如 `docs/epic-{n}.md` - 以下簡稱「Epic 檔案」)
  - `docs/stories/` 中現有的 story 檔案
  - 主要 PRD (以下簡稱「PRD 文件」)
  - 主要架構文件 (以下簡稱「主要架構文件」)
  - 前端架構文件 (以下簡稱「前端架構文件」，若相關)
  - 專案結構指南 (`docs/project-structure.md`)
  - 操作指南文件 (`docs/operational-guidelines.md`)
  - 技術堆疊文件 (`docs/tech-stack.md`)
  - 資料模型文件 (如索引文件中所引用)
  - API 參考文件 (如索引文件中所引用)
  - UI/UX 規格、風格指南、組件指南 (若相關，如索引文件中所引用)
- `bmad-agent/templates/story-tmpl.md` (以下簡稱「Story 範本」)
- `bmad-agent/checklists/story-draft-checklist.md` (以下簡稱「Story 草稿檢查表」)
- 使用者確認繼續進行 story 識別，並在需要時覆寫關於未完成先決 story 的警告。

## 任務執行說明

### 1. 識別下一個待準備的 Story

- 檢閱 `docs/stories/` 以尋找編號最大的 story 檔案。
- **若存在編號最大的 story 檔案 (`{lastEpicNum}.{lastStoryNum}.story.md`):**

  - 驗證其 `Status` 為 'Done' (或等效狀態)。
  - 若非 'Done'，向使用者顯示警告：

    ```
    警告：發現未完成的 story：
    檔案：{lastEpicNum}.{lastStoryNum}.story.md
    狀態：[目前狀態]

    您想：
    1. 查看未完成 story 的詳細資訊 (指示使用者執行此操作，agent 不顯示)
    2. 此時取消建立新的 story
    3. 接受風險並覆寫以草稿形式建立下一個 story

    請選擇一個選項 (1/2/3)：
    ```

  - 僅當使用者選擇選項 3 (覆寫) 或最後一個 story 為 'Done' 時繼續。
  - 若繼續：檢查 `{lastEpicNum}` 的 Epic 檔案中是否有編號為 `{lastStoryNum + 1}` 的 story。若存在且其先決條件 (根據 Epic 檔案) 已滿足，則此為下一個 story。
  - 否則 (找不到 story 或先決條件未滿足)：下一個 story 是下一個 Epic 檔案 (例如 `docs/epic-{lastEpicNum + 1}.md`，然後是 `docs/epic-{lastEpicNum + 2}.md` 等) 中第一個滿足先決條件的 story。

- **若 `docs/stories/` 中不存在任何 story 檔案：**
  - 下一個 story 是 `docs/epic-1.md` (然後是 `docs/epic-2.md` 等) 中第一個滿足先決條件的 story。
- 若找不到符合先決條件的 story，向使用者報告 story 建立受阻，並說明哪些先決條件待處理。中止任務。
- 向使用者宣告已識別的 story：「已識別下一個待準備的 story：{epicNum}.{storyNum} - {Story 標題}」。

### 2. 收集核心 Story 需求 (從 Epic 檔案)

- 針對已識別的 story，開啟其父層 Epic 檔案。
- 提取：確切的標題、完整的目標/使用者 Story 陳述、初始需求清單、所有驗收標準 (ACs) 以及任何預先定義的高階任務。
- 保留此原始 epic 定義範圍的記錄，以供後續偏差分析。

### 3. 為 Dev Agent 收集並綜合深入的技術背景資訊

- <critical_rule>系統性地使用索引文件 (`docs/index.md`) 作為您的主要指南，以探索與目前 story 實作需求相關的所有詳細文件的路徑。</critical_rule>
- 徹底檢閱 PRD 文件、主要架構文件以及前端架構文件 (若是 UI story)。
- 在索引文件和 story 需求的指引下，從以下來源定位、分析並綜合具體的相關資訊：
  - 資料模型文件 (結構、驗證規則)。
  - API 參考文件 (端點、請求/回應結構描述、驗證)。
  - 架構文件中適用的架構模式或組件設計。
  - UI/UX 規格、風格指南、組件指南 (適用於 UI story)。
  - 若版本或組態為此 story 的關鍵，則參考技術堆疊文件中的具體細節。
  - 操作指南文件的相關章節 (例如，story 特定的錯誤處理細微差異、此 story 中處理的資料的安全考量)。
- 目標是收集 Dev Agent 所需的所有必要詳細資訊，以避免他們進行大量搜尋。記錄 epic 與這些詳細資訊之間的任何差異，以進行「偏差分析」。

### 4. 驗證專案結構一致性

- 將 story 的需求和預期的檔案操作與專案結構指南 (以及前端結構，若適用) 進行交叉參照。
- 確保 story 暗示的任何檔案路徑、組件位置或模組名稱與已定義的結構一致。
- 在 story 草稿內的「專案結構備註」部分記錄任何結構衝突、必要的澄清或未定義的組件/路徑。

### 5. 用完整背景資訊填寫 Story 範本

- 建立新的 story 檔案：`docs/stories/{epicNum}.{storyNum}.story.md`。
- 使用 Story 範本來架構檔案。
- 填寫：
  - Story `{EpicNum}.{StoryNum}: {從 Epic 檔案複製的簡短標題}`
  - `Status: Draft`
  - `Story` (來自 Epic 的使用者 Story 陳述)
  - `Acceptance Criteria (ACs)` (來自 Epic，若需要則根據背景資訊調整)
- **`Dev Technical Guidance` 部分 (關鍵)：**
  - 根據所有收集到的背景資訊 (步驟 3 和 4)，嵌入簡潔但關鍵的資訊片段、特定的資料結構、API 端點詳細資訊、對其他文件中*特定章節*的精確引用 (例如，「有關詳細資訊，請參閱 `Data Models Doc#User-Schema-ValidationRules`」)，或簡要說明架構模式如何應用於*此 story*。
  - 若是 UI story，提供與*此 story 元素*相關的組件/風格指南的具體引用。
  - 目標是使此部分成為 Dev Agent 獲取*story 特定*技術背景資訊的主要來源。
- **`Tasks / Subtasks` 部分：**
  - 根據收集到的背景資訊，產生 Dev Agent 完成 story 必須執行的詳細、循序的技術任務和子任務清單。
  - 在適用情況下將任務連結至 ACs (例如，`Task 1 (AC: 1, 3)`)。
- 新增在步驟 4 中發現的專案結構一致性或差異的備註。
- 根據步驟 3 中記錄的差異準備「偏差分析」的內容。
