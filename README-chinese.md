# BMAD 方法 (突破性敏捷 (AI 驅動) 開發方法)

**BETA-V3 是目前開發的重點，代表 BMAD 方法的最新迭代。** 在 `BETA-V3/` 目錄中尋找所有 V3 資源。

如果您想直接開始，這裡是 [V3 設定說明](./BETA-V3/instruction.md)，用於 IDE、WEB 和任務設定。

## BETA-V3：推進 AI 驅動的開發

歡迎來到 **BETA-V3**！此版本代表著重大的演進，建立在 V2 的基礎之上，並引入了一套更精緻、更全面的代理程式、範本、檢查清單和流程。

歡迎試用 - 它是測試版，所以請注意它仍在進行測試和更新，並且有一些重大的 (驚人的改進) 變更。

_先前版本 (`LEGACY-V1` 和 `CURRENT-V2`) 可供歷史參考，但不再積極維護。_

## BETA-V3 有哪些新功能？

BETA-V3 引入了幾項關鍵增強功能，以簡化您的 AI 驅動開發生命週期：

- **增強的代理程式角色與階段：** 核心代理程式 (Analyst、PM、Architect) 具有更明確定義的操作階段、輸入和輸出，從而實現更順暢的轉換。
- **新的專業代理程式：**
  - **Design Architect (`4-design-architect.md`):** 一個專門用於具有使用者介面專案的代理程式，在不同模式下處理 UI/UX 規格和前端技術架構。
  - **Technical POSM (`5-posm.md`) (Product Owner & Scrum Master):** 一個具有關鍵新功能的統一代理程式：
    - **Master Checklist Phase:** 根據全面的檢查清單 (`po-master-checklist.txt`) 驗證所有專案文件。
    - **Librarian Phase:** 將大型文件分解為專案 `docs/` 資料夾內細緻、索引化 (`docs/index.md`) 的文件生態系統，針對 AI 代理程式使用和人工導覽進行最佳化 (建議使用 IDE)。
    - **Story Creator Phase:** 使用細緻的文件自主產生詳細、開發人員就緒的 story 檔案。
  - **Release Train Engineer (RTE-Agent) (`6-rte.md`):** 一個關鍵代理程式，用於處理重大的專案中期變更 (pivots、技術問題、遺漏的需求)，分析影響並草擬必要的產出物更新。
- **改進的代理程式互動 (更輕鬆的多問題回答)：** 代理程式現在在一次提出多個問題時會對問題進行編號 (例如，「1.、2a.、2b.」)。這使得使用者可以更輕鬆地針對每個要點進行具體回應，這在透過語音互動時尤其有幫助。
- **增強的 PM 代理程式彈性 (客製化的 Story 細緻度)：** Product Manager (PM) 代理程式在 PRD 產生模式下，現在可以在兩種不同的工作流程情境中運作：
  - **完整的敏捷團隊工作流程：** PM 專注於以成果為基礎的 User Stories，將詳細的技術闡述留給下游的 Architect 和 Scrum Master 角色。
  - **簡化的 PM 到開發工作流程：** PM 採取更「解決方案感知」的立場，產生更細緻的 User Stories 和 Acceptance Criteria。這適用於需要更直接移交給 Scrum Master 然後再到開發的情境，或者當 Architect 的角色更具諮詢性時。
- **IDE Tasks (`BETA-V3/tasks/`):** IDE 代理程式執行特定一次性工作的獨立指令集 (例如，執行檢查清單、建立下一個 story、分割文件)，而不會使主要代理程式定義過於臃腫。[詳見下文](#ide-tasks-v3-專屬)。
- **全面且更新的範本：** 新的和修訂的範本支援擴展的代理程式功能，位於 `BETA-V3/templates/`。
- **詳細的檢查清單：** 新的和更新的檢查清單確保每個階段的品質和完整性，位於 `BETA-V3/checklists/`。
- **簡化的工作流程與文件重點：** 一個更明確、迭代的工作流程，包含所有代理程式，並強調建立和維護一個強大、細緻且索引化的文件結構 (`docs/`) 以支援開發。
- **明確的代理程式交接：** 更清楚地說明每個代理程式產生的內容以及後續代理程式期望的輸入。
- **最佳化的 IDE 代理程式：** IDE 代理程式模式 (`BETA-V3/ide-agent-modes/`) 針對大小進行了最佳化 (<6k tokens)，以實現更廣泛的 IDE 相容性。
- **Web UI 一致性：** 代理程式指令和範本設計用於 Web UI (指令和檔案位於 `BETA-V3/web-agent-modes/`) 和 IDE 自訂模式。

## 指導原則 (V3)

- **環境建議與工作流程：**
  - **初始規劃 (代理程式 0-5) 偏好使用 Web UI：** 對於初始構思、研究、PRD 建立、架構設計、UI/UX 規格和初始文件驗證 (Analyst、PM、Architect、Design Architect 和 POSM Master Checklist 階段)，Web UI (Gems/Custom GPTs) 非常有效，並且對於這些任務的迭代、對話性質通常更具成本效益。
  - **POSM - Librarian & Story Creator：** 雖然 POSM 的 Librarian 階段 (文件分割和索引) 強烈建議在 IDE 中進行，因為涉及檔案系統操作，但 Story Creator 階段*可以*在 Web UI 中完成。然而，IDE 通常更受青睞，因為更容易與現有的 `docs/` 內容和最終的程式碼庫進行交叉引用。
  - **執行與專業任務建議使用 IDE：** 對於開發 (Dev Agent)、詳細的 story 產生 (SM Agent 或 IDE 中的 POSM Story Creator) 以及管理重大的專案中期變更 (pivots、技術問題、遺漏的需求)，具有自訂代理程式模式的 IDE 環境通常更強大且更有效率。核心建議的 IDE 代理程式，尤其是在初始基於 Web 的規劃之後，是 **Dev Agent、SM/Story Creator 和 RTE-Agent**。
- **品質與資訊流 (V3 增強功能)：**
  - **PM Agent** 經過調整，可產生更高品質、更詳細的 Epics 和 User Stories。
  - **SM Agent (或 Story Creator 模式下的 POSM)** 在 story 檔案中包含的內容與 **Dev Agent** 期望的內容之間的一致性有所提高。SM 專注於將必要的上下文詳細資訊直接嵌入到 story 中。
  - **Dev Agent** 被編程為在開始新的 story 時始終查閱標準專案文件，如 `docs/coding-standards.md` 和 `docs/project-structure.md`。這減少了在每個 story 檔案中重複此樣板資訊的需求，使 story 更精簡並專注於特定需求。
- **無需規則 (彈性)：** 代理程式主要參考專案文件 (PRD、架構文件、編碼標準等)，而不是嚴重依賴專有的 AI 平台規則系統。這提高了彈性並減少了平台鎖定。
- **迭代與協作：** 此方法強調逐步、互動的過程，代理程式與使用者協作，在關鍵決策點暫停以獲取輸入和驗證。
- **文件重點 (V3 增強)：** V3 更強調建立一個細緻、索引化的 `docs/` 結構，以改善 AI 代理程式和人工開發人員的上下文。

## 什麼是 BMad 方法？

BMad 方法是一種革命性的方法，將「vibe coding」提升到「Vibe CEOing」。它提供了一個結構化但靈活的框架，使用一組專業的 AI 代理程式來規劃、執行和管理軟體專案。BETA-V3 代表了最新的進展，讓使用者能夠透過從構思到準備好實施的開發人員 story，利用 AI 更快、更便宜、更有效地進行建置。

該方法原則上設計為工具無關，代理程式指令和工作流程可適應各種 AI 平台和 IDE。

加入 [社群討論論壇](https://github.com/bmadcode/BMAD-METHOD/discussions) 以貢獻和發展這些想法。

## 自訂模式與歡迎貢獻

BMAD 社群的意見非常寶貴！我們鼓勵您透過提交 Pull Requests 來貢獻 V3 自訂代理程式模式、新範本或針對 `BETA-V3` 系統量身打造的檢查清單增強功能。

`BETA-V3` 中的自訂代理程式遵循 LLM 提示的最佳實踐 (例如，明確的角色、指令和上下文)，確保它們在各種 AI 平台上有效運作。

## BETA-V3 代理程式概覽

`BETA-V3` 系統具有一組經過優化的專業 AI 代理程式團隊 (詳細資訊請參閱 `BETA-V3/web-agent-modes/` 或 `BETA-V3/ide-agent-modes/`)：

### 0. BMAD 方法顧問 (`0-bmad.md`)

理解和導覽 BMAD 方法 V3 的主要指南，解釋代理程式角色、工作流程、任務和最佳實踐。

### 1. Analyst (`1-analyst.md`)

新/不清楚想法的起點。
**階段：** Brainstorming、深度研究 (廣泛市場/可行性)、專案簡報。
**主要輸出：** **專案簡報 (Project Brief)**。

### 2. Product Manager (PM) (`2-pm.md`)

將想法/簡報轉換為詳細的產品計劃。
**階段：** 深度研究 (專注於策略驗證)、PRD 產生 (Epics、Stories、使用 `prd-tmpl.txt` 和 `pm-checklist.txt` 的技術假設)、產品顧問。
**主要輸出：** **產品需求文件 (PRD)**。

### 3. Architect (`3-architect.md`)

定義整體技術藍圖。
**階段：** 深度研究提示產生、架構建立 (使用 `architecture-tmpl.txt` 和 `architect-checklist.txt`)、主要架構師諮詢。
**主要輸出：** **技術架構文件**。

### 4. Design Architect (`4-design-architect.md`)

專精於 UI/UX 和前端技術策略。
**模式：** UI/UX 規格 (使用 `front-end-spec-tmpl.txt`)、前端架構 (使用 `front-end-architecture-tmpl.txt` 和 `frontend-architecture-checklist.txt`)、AI 前端產生提示。
**主要輸出：** **UI/UX 規格 (UI/UX Spec)**、**前端架構文件 (Frontend Architecture Doc)**。

### 5. Technical POSM (`5-posm.md`)

確保文件品質並為開發做準備。
**階段：** Master Checklist (使用 `po-master-checklist.txt`)、Librarian (建立 `docs/` 結構和 `index.md`)、Story Creator (使用 `story-tmpl.txt`)。
**主要輸出：** **檢查清單報告 (Checklist Report)**、有組織的細緻文件、**Story 檔案 (Story Files)**。

### 6. Release Train Engineer (RTE-Agent) (`6-rte.md`)

管理重大的專案中期變更和 pivots。
**流程：** 使用 `rte-checklist.md` 進行影響分析、評估路徑、草擬產出物更新。
**主要輸出：** **Sprint 變更提案 (Sprint Change Proposal)** (包含分析和建議的編輯)。

### 7. Developer Agents (例如，`dev-agent.md`)

(特定提示位於 `BETA-V3/ide-agent-modes/`)
根據 story 實作功能，遵守架構和標準 (`coding-standards.md`、`project-structure.md` 等)。

## IDE Tasks (V3 專屬！)

位於 `BETA-V3/tasks/`，這些獨立的指令集允許 IDE 代理程式按需執行特定的一次性工作，減少了對過於複雜代理程式模式的需求。

**目的：**

- **減少代理程式臃腫：** 避免將很少使用的指令添加到主要代理程式中。
- **按需功能：** 指示任何有能力的 IDE 代理程式透過提供任務檔案內容來執行任務。
- **多功能性：** 處理特定功能，如執行檢查清單、建立 story、分割文件、索引庫等。

將任務視為可由您的主要 IDE 代理程式呼叫的專業迷你代理程式。

## BETA-V3 逐步流程 (典型流程)

1.  **Analyst (`1-analyst.md`):** (選擇性) -> **專案簡報 (Project Brief)**。
2.  **PM (`2-pm.md`):** 專案簡報/想法 -> **PRD** (Epics, Stories)。
3.  **Architect (`3-architect.md`):** PRD -> **系統架構文件 (System Architecture Document)**。
4.  **Design Architect (`4-design-architect.md`):** (若有 UI) PRD, 系統架構 -> **UI/UX 規格 (UI/UX Spec)**, **前端架構文件 (Frontend Architecture Doc)**。
5.  **POSM (`5-posm.md`) - Master Checklist Phase:** 驗證文件 -> **Master Checklist 報告 (Master Checklist Report)**。
6.  _(使用者/代理程式根據 POSM 報告套用變更)_。
7.  **POSM (`5-posm.md`) - Librarian Phase:** 處理更新後的文件 -> 細緻的 `docs/` 檔案和 `index.md`。
8.  **POSM (`5-posm.md`) - Story Creator Phase:** -> **開發人員 Story 檔案 (Developer Story Files)**。
9.  **Developer Agents:** 實作 story。
10. **(若發生重大問題)：RTE-Agent (`6-rte.md`):** -> **Sprint 變更提案 (Sprint Change Proposal)** (包含分析和建議的編輯)。套用編輯或在需要重新規劃時移交。
11. **持續諮詢：** Architect 和 PM 提供持續支援。

_這是一個指南；此方法是迭代的。_

## 工具與設定 (BETA-V3)

- **範本：** `BETA-V3/templates/`
- **檢查清單：** `BETA-V3/checklists/`
- **Web UI 代理程式：** `BETA-V3/web-agent-modes/` (與 Gemini Gems/Custom GPTs 一起使用。附加相關的範本/檢查清單)。
- **IDE 代理程式：** `BETA-V3/ide-agent-modes/` (針對大小進行了最佳化 (<6k tokens)，以實現更廣泛的 IDE 相容性)。
- **任務：** `BETA-V3/tasks/` (供 IDE 使用)。
- **說明：** 詳細資訊請參閱 [設定說明](./BETA-V3/instruction.md)。

## 歷史版本 (V1 & V2)

_V1/V2 的簡要摘要可以保留在此處，或者如果需要，可以移至單獨的歷史文件。_
_(原始 V2 部分，如「V2 有哪些新功能？」、「完整示範演練」(參考 V2 示範)、「無需規則！」、「IDE 代理程式整合」(參考 V2 路徑) 等將被審查、總結或刪除，以避免與 V3 混淆)。_

## 授權

[授權](./LICENSE)

## 貢獻

有興趣改進 BMAD 方法 BETA-V3 嗎？請參閱我們的 [貢獻指南](CONTRIBUTING.md)。
