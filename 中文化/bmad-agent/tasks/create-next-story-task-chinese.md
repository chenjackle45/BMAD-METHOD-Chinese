# 建立下一個 Story 任務

## 目的

根據專案進度和 epic 定義，識別下一個合乎邏輯的 story，然後使用 `Story Template` 準備一份全面、獨立且可執行的 story 檔案。此任務確保 story 充實了所有必要的技術背景、需求和驗收標準，使其可供 Developer Agent 高效實作，並盡可能減少額外研究的需求。

## 此任務的輸入

- 存取專案的文件儲存庫，特別是：
  - `docs/index.md` (以下簡稱 "Index Doc")
  - 所有 Epic 檔案 (例如 `docs/epic-{n}.md` - 以下簡稱 "Epic Files")
  - `docs/stories/` 中現有的 story 檔案
  - 主要 PRD (以下簡稱 "PRD Doc")
  - 主要架構文件 (以下簡稱 "Main Arch Doc")
  - 前端架構文件 (以下簡稱 "Frontend Arch Doc"，若相關)
  - 專案結構指南 (`docs/project-structure.md`)
  - 操作指南文件 (`docs/operational-guidelines.md`)
  - 技術堆疊文件 (`docs/tech-stack.md`)
  - 資料模型文件 (如 Index Doc 中所引用)
  - API 參考文件 (如 Index Doc 中所引用)
  - UI/UX 規格、風格指南、組件指南 (若相關，如 Index Doc 中所引用)
- `docs/templates/story-template.md` (以下簡稱 "Story Template")
- `docs/checklists/story-draft-checklist.txt` (以下簡稱 "Story Draft Checklist")
- 使用者確認繼續進行 story 識別，並在需要時覆寫關於未完成先決 story 的警告。

## 任務執行說明

### 1. 識別下一個待準備的 Story

- 檢閱 `docs/stories/` 以尋找編號最大的 story 檔案。
- **若存在編號最大的 story 檔案 (`{lastEpicNum}.{lastStoryNum}.story.md`):**

  - 驗證其 `Status` 是否為 'Done' (或等效狀態)。
  - 若非 'Done'，向使用者顯示警告：

    ```
    警告：發現未完成的 story：
    檔案：{lastEpicNum}.{lastStoryNum}.story.md
    狀態：[current status]

    您想：
    1. 查看未完成的 story 詳細資訊 (指示使用者執行此操作，agent 不顯示)
    2. 此時取消建立新的 story
    3. 接受風險並覆寫以草稿形式建立下一個 story

    請選擇一個選項 (1/2/3)：
    ```

  - 僅當使用者選擇選項 3 (覆寫) 或最後一個 story 為 'Done' 時繼續。
  - 若繼續：檢查 `{lastEpicNum}` 的 Epic File 中是否有編號為 `{lastStoryNum + 1}` 的 story。若存在且其先決條件 (根據 Epic File) 已滿足，則此為下一個 story。
  - 否則 (未找到 story 或先決條件未滿足)：下一個 story 是下一個 Epic File (例如 `docs/epic-{lastEpicNum + 1}.md`，然後是 `{lastEpicNum + 2}.md` 等) 中第一個滿足先決條件的 story。

- **若 `docs/stories/` 中不存在 story 檔案：**
  - 下一個 story 是 `docs/epic-1.md` (然後是 `docs/epic-2.md` 等) 中第一個滿足先決條件的 story。
- 若未找到符合條件且滿足先決條件的 story，向使用者報告 story 建立受阻，並說明哪些先決條件待處理。中止任務。
- 向使用者宣告已識別的 story：「已識別下一個待準備的 story：{epicNum}.{storyNum} - {Story Title}」。

### 2. 收集核心 Story 需求 (來自 Epic File)

- 針對已識別的 story，開啟其父 Epic File。
- 提取：確切的標題、完整的目標/使用者故事陳述、初始需求清單、所有驗收標準 (ACs) 以及任何預先定義的高階任務。
- 記錄此原始 epic 定義的範圍，以供後續偏差分析。

### 3. 為 Dev Agent 收集並綜合深入的技術背景

- <critical_rule>系統性地使用 Index Doc (`docs/index.md`) 作為您的主要指南，以探索所有與當前 story 實作需求相關的詳細文件路徑。</critical_rule>
- 徹底檢閱 PRD Doc、Main Arch Doc 和 Frontend Arch Doc (若是 UI story)。
- 在 Index Doc 和 story 需求的指引下，從以下來源定位、分析並綜合特定相關資訊：
  - Data Models Doc (結構、驗證規則)。
  - API Reference Doc (端點、請求/回應結構描述、驗證)。
  - Arch Docs 中適用的架構模式或組件設計。
  - UI/UX 規格、風格指南、組件指南 (適用於 UI story)。
  - 若版本或組態對此 story 至關重要，則從 Tech Stack Doc 中提取特定資訊。
  - Operational Guidelines Doc 的相關章節 (例如，此 story 特有的錯誤處理細微差別、此 story 中處理的資料的安全考量)。
- 目標是收集 Dev Agent 所需的所有必要詳細資訊，以避免他們進行大量搜尋。記錄 epic 與這些詳細資訊之間的任何差異，以進行「偏差分析」。

### 4. 驗證專案結構一致性

- 將 story 的需求和預期的檔案操作與 Project Structure Guide (以及適用的前端結構) 進行交叉參照。
- 確保 story 暗示的任何檔案路徑、組件位置或模組名稱與定義的結構一致。
- 在 story 草稿的「專案結構備註」部分記錄任何結構衝突、必要的澄清或未定義的組件/路徑。

### 5. 以完整背景資訊填寫 Story Template

- 建立新的 story 檔案：`docs/stories/{epicNum}.{storyNum}.story.md`。
- 使用 Story Template 來組織檔案結構。
- 填寫：
  - Story `{EpicNum}.{StoryNum}: {從 Epic File 複製的簡短標題}`
  - `Status: Draft`
  - `Story` (來自 Epic 的使用者故事陳述)
  - `Acceptance Criteria (ACs)` (來自 Epic，若需要則根據背景資訊進行調整)
- **`Dev Technical Guidance` 部分 (關鍵)：**
  - 根據所有收集到的背景資訊 (步驟 3 和 4)，嵌入簡潔但關鍵的資訊片段、特定的資料結構、API 端點詳細資訊、對其他文件中*特定章節*的精確引用 (例如，「詳情請參閱 `Data Models Doc#User-Schema-ValidationRules`」)，或簡要說明架構模式如何應用於*此 story*。
  - 若是 UI story，提供與*此 story 元素*相關的組件/風格指南的特定引用。
  - 目標是使此部分成為 Dev Agent 獲取*story 特定*技術背景的主要來源。
- **`Tasks / Subtasks` 部分：**
  - 根據收集到的背景資訊，產生一份詳細、依序的技術任務和子任務清單，供 Dev Agent 完成 story。
  - 在適用情況下將任務連結至 ACs (例如，`Task 1 (AC: 1, 3)`)。
- 新增在步驟 4 中發現的專案結構一致性或差異的備註。
- 根據步驟 3 中記錄的差異準備「偏差分析」的內容。
