# BMAD 方法 (敏捷 (AI 驅動) 開發的突破性方法)

**歡迎來到 BMAD 方法！本儲存庫記錄了 V2 方法論 (穩定版) 並介紹了 BETA-V3 (實驗版)。**

- **穩定版本 (V2):** 資源位於 `CURRENT-V2/`。這是推薦用於一般用途的版本。
- **實驗性 Beta (V3):** 資源位於 `BETA-V3/`。包含目前正在測試的新功能和代理程式。

快速連結：

- [V2 設定說明](./CURRENT-V2/instructions.md) (適用於 IDEs/Gems/GPTs)
- [BETA-V3 設定說明](./BETA-V3/instruction.md) (適用於 IDEs/Web/Tasks)
- [社群討論論壇](https://github.com/bmadcode/BMAD-METHOD/discussions)
- [貢獻指南](CONTRIBUTING.md)
- [授權條款](./LICENSE)

_舊版 V1 已封存。_

## BMAD 方法是什麼？

BMAD 方法是一種革命性的方法，將「vibe coding」提升至 **「Vibe CEOing」**。不同於純粹 vibe coding 用於快速原型製作的自發性，此方法提供了一個結構化且具彈性的框架，可利用一組專業的 AI 代理程式來規劃、執行和管理軟體專案。它透過在整個產品開發生命週期中利用 AI 驅動的流程，協助您更快、更經濟且更有效地建構。

**V2 版本 (位於 `CURRENT-V2/`)** 建立了一個穩健的工作流程，具有明確的代理程式角色、標準化範本和全面的檢查表。**BETA-V3 (位於 `BETA-V3/`)** 在此基礎上建構，改進流程並引入目前正在評估的新功能。

此全面、逐步的方法透過以下方式將產品概念轉化為完全實作的應用程式：

1.  將開發流程結構化為基於不同 AI 角色的階段。
2.  在每個階段產生詳細、高品質的交付成果。
3.  使用具有明確交接的循序工作流程。
4.  使 AI 能夠根據明確定義的規格編寫應用程式程式碼。

## 指導原則

這些原則適用於所有版本，V3 則強化了某些方面：

- **環境建議：** 一般而言，初期規劃階段 (分析師、PM、早期架構師工作) 通常非常適合 Web UI (Gems/自訂 GPT)，因為它們具有對話性質且符合成本效益。技術實作、檔案管理 (如 V3 的 POSM Librarian)、專業任務 (如 V3 的 RTE-Agent 或 IDE Tasks) 和開發通常更適合具有自訂代理程式支援的 IDE 環境。
- **無需規則 (彈性)：** 代理程式主要參考專案文件 (PRD、架構文件、編碼標準等)，而非嚴重依賴專有 AI 平台的規則。
- **迭代與協作：** 強調逐步、互動的流程，代理程式與使用者協作，在關鍵決策點暫停以取得輸入和驗證。
- **文件焦點 (V3 強化)：** V3 更強調建立一個精細、索引化的 `docs/` 結構，以改善 AI 代理程式和人類開發人員的上下文。

## 代理程式概覽 (V2 核心代理程式)

`CURRENT-V2/` 系統擁有一組專業的 AI 代理程式。請參閱 `CURRENT-V2/agents/` 以了解 IDE 模式，並參閱 `CURRENT-V2/gems-and-gpts/` 以了解 Web UI 版本。

### 分析師 (業務分析師、研究助理、腦力激盪教練)

新概念或不明確概念的起點。將概念轉化為結構化的**專案簡報 (Project Brief)**。

### 產品經理 (PM)

將概念/簡報轉化為詳細的產品計畫。建立包含 Epics 和 User Stories 的**產品需求文件 (PRD)**。V2 包含用於驗證的檢查表。

### 架構師

根據 PRD 定義整體技術藍圖。建立**技術架構文件**。

### 產品負責人 (PO)

在開發開始前，根據檢查表驗證 MVP 計畫 (PRD、架構、Epics)。

### Scrum Master (SM)

技術橋樑，根據核准的計畫，一次一個地產生詳細、可執行的 **Story 檔案**，準備進行開發。

### 開發者代理程式 (Dev)

根據指派的 Story 檔案實作功能，並遵守專案架構和標準。

## BETA-V3 中的新增/強化代理程式

BETA-V3 (位於 `BETA-V3/`) 引入了新角色並強化了現有角色：

### 0. BMAD 方法顧問 (`0-bmad.md`)

專門的指南，解釋 V3 概念，包括新的代理程式、IDE Tasks 和工作流程的細微差異。

### 4. 設計架構師 (`4-design-architect.md`)

(若專案有 UI 則參與)
專精於 UI/UX (**UI/UX 規格**) 和前端技術策略 (**前端架構文件**)。

### 5. 技術 POSM (`5-posm.md`)

以強化的功能取代 V2 中獨立的 PO 和 SM：
**階段：**

1.  **主檢查表 (Master Checklist)：** 驗證所有規劃/設計文件。
2.  **Librarian：** 建立精細的 `docs/` 結構和 `index.md` (建議使用 IDE)。
3.  **Story Creator：** 產生開發人員就緒的 **Story 檔案**。

### 6. Release Train Engineer (RTE-Agent) (`6-rte.md`)

管理專案中期的重大變更/轉向。使用 `rte-checklist.md` 進行分析並草擬交付成果更新，輸出**Sprint 變更提案 (Sprint Change Proposal)**。

_(注意：V3 PM 和架構師代理程式也具有精煉的階段，並使用更新的 V3 範本/檢查表。)_

## 逐步流程 (V2 典型流程)

1.  **分析師：** (選擇性) -> **專案簡報 (Project Brief)**。
2.  **PM：** 專案簡報/概念 -> **PRD** (Epics, Stories)。
3.  **架構師：** PRD -> **系統架構文件**。
4.  **PO：** 驗證 PRD、架構、Epics -> **驗證報告/核准**。
5.  **SM：** 核准的計畫 -> 產生 **Story 檔案** (一次一個)。
6.  **開發者代理程式：** 實作核准的 story。
7.  重複 5 和 6 直到 MVP 完成。
8.  **持續諮詢：** 架構師和 PM 提供支援。

## BETA-V3 流程修改

BETA-V3 改進了流程，特別是在初期規劃之後：

- 如果需要 UI，則在架構師之後加入**設計架構師**。
- **POSM** 取代 PO 和 SM：
  - 規劃文件 (PRD、架構文件、設計架構文件) 準備就緒後，**POSM (主檢查表階段)** 會驗證所有內容。
  - 驗證/更新後，**POSM (Librarian 階段)** 會組織 `docs/` 結構。
  - 然後，**POSM (Story Creator 階段)** 可以 (但不建議從 IDE) 為 stories 資料夾中的整個 epic 逐一產生 stories，供開發代理程式取用。
- 如果發生重大問題或轉向，可以在 story 開發開始後的任何時間點引入 **RTE-Agent** - RTE (Release Train Engineer) 可以協助找出需要變更的內容以及如何進行，並協助更新所有交付成果 - 由於我們處於專案中期，最好從 IDE 執行。

(有關詳細流程，請參閱 `BETA-V3` 代理程式定義)。

## IDE Tasks (BETA-V3 功能)

位於 `BETA-V3/tasks/`，這些獨立的指令集允許 IDE 代理程式 (在 V3 模式下) 按需執行特定的一次性工作 (例如，執行檢查表、建立下一個 story、分割文件)，從而減少對複雜代理程式模式的需求。

## 工具與設定

### V2 (穩定版 - 位於 `CURRENT-V2/`)

- **範本：** `CURRENT-V2/docs/templates/`
- **檢查表：** `CURRENT-V2/docs/checklists/`
- **Web UI 代理程式 (Gems/GPTs)：** `CURRENT-V2/gems-and-gpts/`
- **IDE 代理程式：** `CURRENT-V2/agents/`
- **設定說明：** `CURRENT-V2/instructions.md`

### BETA-V3 (實驗版 - 位於 `BETA-V3/`)

- **範本：** `BETA-V3/templates/`
- **檢查表：** `BETA-V3/checklists/`
- **Web UI 代理程式：** `BETA-V3/web-agent-modes/`
- **IDE 代理程式：** `BETA-V3/ide-agent-modes/`
- **Tasks：** `BETA-V3/tasks/`
- **設定說明：** `BETA-V3/instruction.md`

## 示範演練

BMAD 方法互動式方法的有效性在 [V2 影片演練](https://youtu.be/p0barbrWgQA?si=PD1RyWNVDpdF3QJf) 中得到了展示。[`V2-FULL-DEMO-WALKTHROUGH`](./V2-FULL-DEMO-WALKTHROUGH/demo.md) 資料夾包含完整的文字記錄和交付成果。

計劃推出 BETA-V3 的特定演練以及多個範例專案。
