# BMAD 知識庫

## 主題索引

- [BMAD 方法 - 核心理念](#bmad-method---core-philosophy)
- [BMAD 方法 - 敏捷方法論概覽](#bmad-method---agile-methodologies-overview)
  - [敏捷的核心原則](#core-principles-of-agile)
  - [敏捷的關鍵實務](#key-practices-in-agile)
  - [敏捷的效益](#benefits-of-agile)
- [BMAD 方法 - 與敏捷原則的類比](#bmad-method---analogies-with-agile-principles)
- [BMAD 方法 - 工具與資源位置](#bmad-method---tooling-and-resource-locations)
- [BMAD 方法 - 社群與貢獻](#bmad-method---community-and-contributions)
  - [授權](#licensing)
- [BMAD 方法 - 精神與最佳實務](#bmad-method---ethos--best-practices)
- [代理人角色](#agent-roles)
- [導覽 BMAD 工作流程 - 初步指引](#navigating-the-bmad-workflow---initial-guidance)
  - [開始您的專案 - Analyst 或 PM？](#starting-your-project---analyst-or-pm)
  - [理解 Epics - 單一或多個？](#understanding-epics---single-or-multiple)
- [建議的代理人參與順序（典型流程）](#suggested-order-of-agent-engagement-typical-flow)
- [處理重大變更](#handling-major-changes)
- [IDE 與 UI 使用 - 一般建議](#ide-vs-ui-usage---general-recommendations)
  - [概念與規劃階段](#conceptual-and-planning-phases)
  - [技術設計、文件管理與實施階段](#technical-design-documentation-management--implementation-phases)
  - [BMAD 方法檔案](#bmad-method-files)
- [利用 IDE 任務提升效率](#leveraging-ide-tasks-for-efficiency)
  - [IDE 任務的目的](#purpose-of-ide-tasks)
  - [任務功能範例](#examples-of-task-functionality)

## BMAD 方法 - 核心理念

**聲明：**「Vibe CEO'ing」是關於擁抱混亂，像一位擁有無限資源和單一願景的 CEO 一樣思考，並利用 AI 作為您的高效能團隊，以快速實現宏偉目標。BMAD 方法（Breakthrough Method of Agile (ai-driven) Development），目前處於其 V3 發行預覽版「Bmad Agent」，將「vibe coding」提升至進階專案規劃，提供一個結構化且靈活的框架，以使用專業 AI 代理人團隊來規劃、執行和管理軟體專案。

**詳細說明：**

- 專注於宏偉目標和快速迭代。
- 利用 AI 作為力量倍增器。
- 以積極主動的心態適應並克服障礙。

## BMAD 方法 - 敏捷方法論概覽

### 敏捷的核心原則

- 個體與互動高於流程與工具。
- 可運作的軟體高於詳盡的文件。
- 客戶協作高於合約協商。
- 回應變化高於遵循計畫。

### 敏捷的關鍵實務

- 迭代式開發：以短週期（sprints）進行建構。
- 漸進式交付：發布產品的功能性部分。
- 每日站立會議：簡短的團隊同步會議。
- 回顧會議：定期檢討以改進流程。
- 持續回饋：來自利害關係人的持續意見。

### 敏捷的效益

- 提升彈性：適應變動需求的能力。
- 加速上市時間：更快交付有價值的功能。
- 改善品質：持續測試與回饋循環。
- 強化利害關係人參與：與使用者/客戶密切協作。
- 提升團隊士氣：賦權且自組織的團隊。

## BMAD 方法 - 與敏捷原則的類比

BMAD 方法，雖然在其以 AI 驅動的「Vibe CEO'ing」方法中獨樹一幟，但與敏捷方法論共享基礎的相似之處：

- **個體與互動高於流程與工具 (Agile) vs. Vibe CEO 與 AI 團隊 (BMAD)：**

  - **Agile：** 強調技術嫻熟的個體與有效溝通的重要性。
  - **BMAD：** 「Vibe CEO」（您）主動指導並與 AI 代理人互動，將其視為高效能團隊。這種互動的品質和清晰的指令（「CLEAR_INSTRUCTIONS」、「KNOW_YOUR_AGENTS」）至關重要，呼應了 Agile 對人為因素的重視。

- **可運作的軟體高於詳盡的文件 (Agile) vs. 快速迭代與高品質產出 (BMAD)：**

  - **Agile：** 優先快速交付可運作的軟體。
  - **BMAD：** 強調「START_SMALL_SCALE_FAST」和「ITERATIVE_REFINEMENT」。雖然「DOCUMENTATION_IS_KEY」對於良好的輸入（briefs, PRDs）至關重要，但目標是利用 AI 快速產生可運作的組件或解決方案。重點在於快速實現宏偉目標。

- **客戶協作高於合約協商 (Agile) vs. Vibe CEO 作為最終仲裁者 (BMAD)：**

  - **Agile：** 涉及來自客戶的持續回饋。
  - **BMAD：** 「Vibe CEO」扮演主要利害關係人和品質控管的角色（「QUALITY_CONTROL」、「STRATEGIC_OVERSIGHT」），持續檢視和改進 AI 的產出，就像一位高度參與的客戶。

- **回應變化高於遵循計畫 (Agile) vs. 擁抱混亂與適應 (BMAD)：**

  - **Agile：** 重視對新需求的適應性和回應能力。
  - **BMAD：** 明確鼓勵「EMBRACE_THE_CHAOS」、「ADAPT & EXPERIMENT」，並承認「ITERATIVE_REFINEMENT」意味著它「不是一個線性過程」。這直接反映了 Agile 的彈性。

- **迭代式開發與漸進式交付 (Agile) vs. 基於 Story 的實施與階段性價值 (BMAD)：**

  - **Agile：** 工作被分解為 sprints，漸進式地交付價值。
  - **BMAD：** 專案被分解為 Epics 和 Stories，「Developer Agents」一次實施一個 story。Epics 代表「重要的、可部署的價值增量」，與漸進式交付一致。

- **持續回饋與回顧會議 (Agile) vs. 迭代式改進與品質控管 (BMAD)：**
  - **Agile：** 團隊定期反思並調整流程。
  - **BMAD：** 「Vibe CEO」持續檢視產出（「QUALITY_CONTROL」）並指導「ITERATIVE_REFINEMENT」，其功能類似於回饋循環和流程改進。

## BMAD 方法 - 工具與資源位置

有效使用 BMAD 方法有賴於理解關鍵工具、組態和資訊資源的位置及其使用方式。該方法原則上設計為工具無關，其代理人指令和工作流程可適應各種 AI 平台和 IDE。

- **BMAD 知識庫：** 本文件（[`bmad-agent/data/bmad-kb.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:101)）作為理解 BMAD 方法、其原則、代理人角色和工作流程的中央儲存庫。
- **Orchestrator Agents：** V3 的一個關鍵特性是 Orchestrator 代理人（例如「BMAD」），這是一個能夠體現任何專業代理人角色的主代理人。
  - **Web Agent Orchestrator：**
    - **設定：** 使用由 [`build-agent-cfg.js`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:104) 設定的 Node.js 建置腳本（[`build-bmad-web-orchestrator.js`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:104)）。
    - **流程：** 將來自 `asset_root`（例如 [`./bmad-agent/`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:105)）的資產（personas, tasks, templates, checklists, data）整合到 `build_dir`（例如 [`./bmad-agent/build/`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:105)）。
    - **輸出：** 產生打包的資產檔案（例如 [`personas.txt`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:106)、[`tasks.txt`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:106)）、一個 [`agent-prompt.txt`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:106)（來自 `orchestrator_agent_prompt`）和一個 [`agent-config.txt`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:106)（來自像 [`web-bmad-orchestrator-agent-cfg.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:106) 這樣的 `agent_cfg`）。
    - **用法：** [`agent-prompt.txt`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:107) 用於主要的自訂 Web 代理人指令集（例如 Gemini 2.5 Gem 或 OpenAI Custom GPT），其他建置檔案則作為知識/檔案附加。
  - **IDE Agent Orchestrator（[`ide-bmad-orchestrator.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:108)）：**
    - **設定：** 無需建置步驟即可運作，動態載入其組態。
    - **組態（[`ide-bmad-orchestrator-cfg.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:110)）：** 包含一個 `Data Resolution` 區段（定義 personas, tasks 等資產的基本路徑）和 `Agent Definitions`（Title, Name, Customize, Persona file, Tasks）。
    - **操作：** 載入其組態，列出可用的 personas，並在使用者請求時，透過載入其 persona 檔案並套用自訂設定來體現所選的代理人。
- **獨立 IDE 代理人：**
  - 針對 IDE 環境（例如 Windsurf, Cursor）進行優化，通常少於 6K 字元（例如 [`dev.ide.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:113)、[`sm.ide.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:113)）。
  - 可以直接參考並執行 tasks。
- **代理人組態檔案：**
  - [`web-bmad-orchestrator-agent-cfg.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:116)：定義 Web Orchestrator 可以體現的代理人，包括對 personas, tasks, checklists 和 templates 的參考（例如 `personas#pm`、`tasks#create-prd`）。
  - [`ide-bmad-orchestrator-cfg.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:117)：設定 IDE Orchestrator，定義 `Data Resolution` 路徑（例如 `(project-root)/bmad-agent/personas`）以及帶有 persona 檔案名稱（例如 [`analyst.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:117)）和 task 檔案名稱（例如 [`create-prd.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:117)）的代理人定義。
  - [`web-bmad-orchestrator-agent.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:118)：Web Orchestrator 的主要提示。
  - [`ide-bmad-orchestrator.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:119)：IDE Orchestrator 代理人的主要提示/定義。
- **Task 檔案：**
  - 位於 [`bmad-agent/tasks/`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:121)（有時也位於 [`bmad-agent/checklists/`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:121) 中，用於類似 checklist 的 tasks）。
  - 特定工作的獨立指令集（例如 [`create-prd.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:122)、[`checklist-run-task.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:122)）。
  - 減少代理人臃腫並為任何有能力的代理人提供隨選功能。
- **核心代理人定義 (Personas)：**
  - 檔案（通常是 `.md`）定義不同代理人的核心個性和指令。
  - 位於 [`bmad-agent/personas/`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:126)（例如 [`analyst.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:126)、[`pm.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:126)）。
- **專案文件（產出）：**
- **Project Briefs：** 由 Analyst 代理人產生。
- **Product Requirements Documents (PRDs)：** 由 PM 代理人產生，包含 epics 和 stories。
- **UX/UI 規格書與架構文件：** 由 Design Architect 和 Architect 代理人建立。
- **POSM 代理人**對於組織和管理這些文件至關重要。
- **Templates：** briefs, PRDs, checklists 等的標準化格式，可能儲存在 [`bmad-agent/templates/`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:132) 中。
- **資料目錄（[`bmad-agent/data/`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:133)）：** 儲存代理人的持久性資料、知識庫（如此知識庫）和其他關鍵資訊。

## BMAD 方法 - 社群與貢獻

BMAD 方法仰賴社群參與和協作改進而蓬勃發展。

- **參與方式：**
  - **GitHub Discussions：** 討論該方法潛在想法、使用案例、新增功能和增強功能的主要平台。
  - **回報錯誤：** 如果您發現錯誤，請先檢查現有問題。如果尚未回報，請提供詳細的重現步驟，以及任何相關的日誌或螢幕截圖。
  - **建議功能：** 檢查現有問題和討論。詳細說明您的功能想法及其潛在價值。
- **貢獻流程 (Pull Requests)：**
  1.  Fork 儲存庫。
  2.  為您的 feature 或 bugfix 建立一個新分支（例如 `feature/your-feature-name`）。
  3.  進行變更，遵守現有的程式碼風格和慣例。為複雜的邏輯撰寫清晰的註解。
  4.  執行任何測試或 linting 以確保品質。
  5.  使用清晰、描述性的訊息提交您的變更（參考專案的 commit 訊息慣例，通常位於 [`docs/commit.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:148)）。
  6.  將您的分支推送到您的 fork。
  7.  針對原始儲存庫的 main 分支開啟一個 Pull Request。
- **行為準則：** 所有參與者都應遵守專案的行為準則。
- **貢獻授權：** 透過貢獻，您同意您的貢獻將根據與專案相同的授權（MIT License）進行授權。

### 授權

BMAD 方法及其相關文件和軟體是根據 **MIT License** 發布的。

Copyright (c) 2025 Brian AKA BMad AKA Bmad Code

特此免費授予任何獲得本軟體及相關文件檔案（以下簡稱「軟體」）副本的人士權限，可以不受限制地處理本軟體，包括但不限於使用、複製、修改、合併、發布、散布、再授權和/或銷售本軟體副本的權利，並允許獲得本軟體的人士在遵守以下條件的情況下這樣做：

上述版權聲明和本許可聲明應包含在本軟體的所有副本或主要部分中。

本軟體乃以「現狀」提供，不附帶任何明示或暗示的保證，包括但不限於適銷性、特定目的適用性和不侵權的保證。在任何情況下，作者或版權所有者均不對任何索賠、損害或其他責任負責，無論是在合約訴訟、侵權行為或其他方面，即使是由於軟體或軟體的使用或其他交易引起或與之相關。

## BMAD 方法 - 精神與最佳實務

- **CORE_ETHOS：** 您是「Vibe CEO」。像一位擁有無限資源和單一願景的 CEO 一樣思考。您的 AI 代理人是您的高效能團隊。您的工作是指導、改進並確保品質，以實現您的宏偉目標。該方法將「vibe coding」提升至進階專案規劃。
- **MAXIMIZE_AI_LEVERAGE：** 驅動 AI。要求更多。挑戰其產出。迭代。
- **QUALITY_CONTROL：** 您是品質的最終仲裁者。檢視所有產出。
- **STRATEGIC_OVERSIGHT：** 維護高層次的願景。確保代理人產出一致。
- **ITERATIVE_REFINEMENT：** 預期會重新檢視步驟。這不是一個線性過程。
- **CLEAR_INSTRUCTIONS：** 您的請求越精確，AI 的產出就越好。
- **DOCUMENTATION_IS_KEY：** 良好的輸入（briefs, PRDs）帶來良好的產出。POSM 代理人對於組織這些至關重要。
- **KNOW_YOUR_AGENTS：** 理解每個代理人的角色（請參閱 [代理人角色與職責](#agent-roles-and-responsibilities) 或以下內容）。如果您正在使用 Orchestrator 代理人，這包括理解其功能。
- **START_SMALL_SCALE_FAST：** 測試概念，然後擴展。
- **EMBRACE_THE_CHAOS：** 開創性方法總是混亂的。適應並克服。
- **ADAPT & EXPERIMENT：** BMAD 方法提供了一個結構，但您可以自由調整其原則、代理人順序或 templates 以適應您的特定專案需求和工作風格。實驗以找出最適合您的方法。**以 BMad 的方式——或您的方式——定義 agile！** 代理人組態允許自訂角色和職責。

## 代理人角色

理解每個代理人獨特的角色和職責是有效導覽 BMAD 工作流程的關鍵。雖然「Vibe CEO」提供整體方向，但每個代理人專注於專案生命週期的不同方面。V3 引入了可以體現這些角色的 Orchestrator 代理人，其組態分別在用於 Web 的 [`web-bmad-orchestrator-agent-cfg.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:182) 和用於 IDE 環境的 [`ide-bmad-orchestrator-cfg.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:182) 中指定。

- **Orchestrator Agent (BMAD)：**

  - **功能：** 主要的 orchestrator，最初為「BMAD」。它可以體現各種專業的代理人 personas。它處理一般的 BMAD 查詢，提供監督，並且在不確定需要哪個專家時作為首選。
  - **Persona 參考：** `personas#bmad` (Web) 或隱含地是 [`ide-bmad-orchestrator.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:187) (IDE) 的核心。
  - **關鍵資料/知識：** 存取 `data#bmad-kb-data` (Web) 作為其知識庫。
  - **類型：**
    - **Web Orchestrator：** 使用腳本建置，利用 Gemini 2.5 或 OpenAI GPTs 等平台的大型上下文視窗。使用打包資產。其行為和可用代理人在 [`web-bmad-orchestrator-agent-cfg.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:190) 中定義。
    - **IDE Orchestrator：** 直接在 Cursor 或 Windsurf 等 IDE 中運作，無需建置步驟，根據其組態（[`ide-bmad-orchestrator-cfg.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:191)）動態載入 persona 和 task 檔案。Orchestrator 本身在 [`ide-bmad-orchestrator.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:191) 中定義。
  - **關鍵特性：** 簡化代理人管理，尤其是在自訂代理人數量受限的環境中。

- **Analyst：**

  - **功能：** 處理研究、需求收集、腦力激盪以及 Project Briefs 的建立。
  - **Web Persona：** `Analyst (Mary)`，persona 為 `personas#analyst`。自訂為「有點萬事通，喜歡用語音表達和抒發情感」。使用 `templates#project-brief-tmpl`。
  - **IDE Persona：** `Analyst (Larry)`，persona 為 [`analyst.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:198)。類似的「萬事通」自訂。用於腦力激盪、深度研究提示產生和 Project Brief 建立的 Tasks 通常在 [`analyst.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:198) persona 本身中定義（「已在 Analyst 記憶中」）。
  - **產出：** `Project Brief`。

- **Product Manager (PM)：**

  - **功能：** 負責建立和維護 Product Requirements Documents (PRDs)、整體專案規劃以及與產品相關的構思。
  - **Web Persona：** `Product Manager (John)`，persona 為 `personas#pm`。使用 `checklists#pm-checklist` 和 `checklists#change-checklist`。採用 `templates#prd-tmpl`。關鍵 tasks 包括 `tasks#create-prd`、`tasks#correct-course` 和 `tasks#create-deep-research-prompt`。
  - **IDE Persona：** `Product Manager (PM) (Jack)`，persona 為 [`pm.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:205)。專注於產生/維護 PRD（[`create-prd.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:205) task）和產品構思/規劃。
  - **產出：** `Product Requirements Document (PRD)`。

- **Architect：**

  - **功能：** 設計系統架構，處理技術設計，並確保技術可行性。
  - **Web Persona：** `Architect (Fred)`，persona 為 `personas#architect`。使用 `checklists#architect-checklist` 和 `templates#architecture-tmpl`。Tasks 包括 `tasks#create-architecture` 和 `tasks#create-deep-research-prompt`。
  - **IDE Persona：** `Architect (Mo)`，persona 為 [`architect.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:212)。自訂為「冷靜、精於計算、代理人團隊的幕後大腦」。產生架構（[`create-architecture.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:212) task），協助規劃 stories（[`create-next-story-task.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:212)），並且可以更新 PO 層級的 epics/stories（[`doc-sharding-task.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:212)）。
  - **產出：** `Architecture Document`。

- **Design Architect：**

  - **功能：** 專注於 UI/UX 規格、前端技術架構，並能為 AI UI 產生服務產生提示。
  - **Web Persona：** `Design Architect (Jane)`，persona 為 `personas#design-architect`。使用 `checklists#frontend-architecture-checklist`、`templates#front-end-architecture-tmpl`（用於 FE 架構）和 `templates#front-end-spec-tmpl`（用於 UX/UI Spec）。Tasks：`tasks#create-frontend-architecture`、`tasks#create-ai-frontend-prompt`、`tasks#create-uxui-spec`。
  - **IDE Persona：** `Design Architect (Millie)`，persona 為 [`design-architect.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:219)。自訂為「風趣隨和，卻是前端設計大師」。協助設計 Web 應用程式，產生 UI 產生提示（[`create-ai-frontend-prompt.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:219) task），規劃 FE 架構（[`create-frontend-architecture.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:219) task），並建立 UX/UI 規格（[`create-uxui-spec.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:219) task）。
  - **產出：** `UX/UI Specification`、`Front-end Architecture Plan`、AI UI 產生提示。

- **Product Owner (PO)：**

  - **功能：** Agile Product Owner，負責驗證文件、確保開發順序、管理 product backlog、執行 master checklists、處理 mid-sprint 重新規劃以及草擬 user stories。
  - **Web Persona：** `PO (Sarah)`，persona 為 `personas#po`。使用 `checklists#po-master-checklist`、`checklists#story-draft-checklist`、`checklists#change-checklist` 和 `templates#story-tmpl`。Tasks 包括 `tasks#story-draft-task`、`tasks#doc-sharding-task`（擷取 epics 並分割架構）和 `tasks#correct-course`。
  - **IDE Persona：** `Product Owner AKA PO (Curly)`，persona 為 [`po.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:226)。被描述為「萬事通」。Tasks 包括 [`create-prd.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:226)、[`create-next-story-task.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:226)、[`doc-sharding-task.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:226) 和 [`correct-course.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:226)。
  - **產出：** User Stories、受管理的 PRD/Backlog。

- **Scrum Master (SM)：**

  - **功能：** 一個技術角色，專注於協助團隊執行 Scrum 流程、促進開發，並經常參與 story 的產生和完善。
  - **Web Persona：** `SM (Bob)`，persona 為 `personas#sm`。被描述為「一位非常技術性的 Scrum Master」。使用 `checklists#change-checklist`、`checklists#story-dod-checklist`、`checklists#story-draft-checklist` 和 `templates#story-tmpl`。Tasks：`tasks#checklist-run-task`、`tasks#correct-course`、`tasks#story-draft-task`。
  - **IDE Persona：** `Scrum Master: SM (SallySM)`，persona 為 [`sm.ide.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:233)。被描述為「超級技術性且注重細節」，專精於「Next Story Generation」（可能利用 [`sm.ide.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:233) persona 的能力）。

- **Developer Agents (DEV)：**
  - **功能：** 一次實作一個 user stories。可以是通用的或專業的。
  - **Web Persona：** `DEV (Dana)`，persona 為 `personas#dev`。被描述為「一位非常技術性的資深軟體開發人員」。
  - **IDE Personas：** 可以存在多種組態，使用 [`dev.ide.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:238) persona 檔案（針對 IDEs 優化至 <6K 字元）。範例：
    - `Frontend Dev (DevFE)`：專精於 NextJS, React, Typescript, HTML, Tailwind。
    - `Dev (Dev)`：大師級通才專家資深全端開發人員。
  - **組態：** 專業代理人可以在 IDE Orchestrator 的 [`ide-bmad-orchestrator-cfg.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:241) 中設定，或為 Web Orchestrator 定義。也提供獨立的 IDE developer agents（例如 [`dev.ide.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:241)）。
  - **使用時機：** 在實施階段，通常在 IDE 內工作。

## 導覽 BMAD 工作流程 - 初步指引

### 開始您的專案 - Analyst 或 PM？

- 如果不確定想法/市場/可行性或需要深入探索，請使用 Analyst。
- 如果概念清晰或您已有 Project Brief，請使用 PM。
- 有關 Analyst 和 PM 的完整詳細資訊，請參閱 [代理人角色與職責](#agent-roles-and-responsibilities)（或本知識庫中的相關章節）。

### 理解 Epics - 單一或多個？

- Epics 代表重要的、可部署的價值增量。
- 對於非小型專案或新的 MVP（不同的功能領域、使用者旅程、分階段推出），通常會有多個 Epics。
- 單一 Epic 可能適用於非常小的 MVPs，或 MVP 後/棕地的 new features。
- PM 協助定義和建構 epics。

## 建議的代理人參與順序（典型流程）

**注意：** 這是一般性指引。BMAD 方法是迭代的；階段/代理人可能會被重新檢視。

1.  **Analyst** - 腦力激盪並建立 project brief。
2.  **PM (Product Manager)** - 使用 brief 產生包含高層級 epics 和 stories 的 PRD。
3.  **Design Architect UX UI Spec for PRD（若專案有 UI）** - 建立前端 UX/UI Specification。
4.  **Architect** - 建立架構並確保我們能在技術上滿足 prd 需求 - 規格需足夠詳細，以使 dev agents 能一致地運作。
5.  **Design Architect（若專案有 UI）** - 建立前端架構並確保我們能在技術上滿足 prd 需求 - 規格需足夠詳細，以使 dev agents 能一致地運作。
6.  **Design Architect（若專案有 UI）** - （選用）建立提示以從 Lovable 或 Vercel 的 V0 等 AI 服務產生 UI。
7.  **PO**：驗證文件是否一致，順序是否合理，執行最終的 master checklist。如果發生重大變更，PO 也可以協助中途開發重新規劃或修正方向。
8.  **PO 或 SM**：一次產生一個 Stories（或多個，但不建議）- 這通常在 Developer Agents 完成每個 story 後於 IDE 中進行。
9.  **Developer Agents**：一次實作一個 Stories。您可以打造不同的專業 Developer Agents，或使用通用的 developer agent。建議建立專業的 developer agents 並在 `ide-bmad-orchestrator-cfg` 中設定它們。

## 處理重大變更

重大變更是宏偉專案中固有的部分。BMAD 方法透過其迭代性質和特定的代理人角色來應對這種情況：

- **設計即迭代：** 整個 BMAD 工作流程建立在「ITERATIVE_REFINEMENT」之上。隨著新資訊的出現或需求的演變，預期會重新檢視先前的步驟和代理人。它「不是一個線性過程」。
- **擁抱與適應：** 核心精神包括「EMBRACE_THE_CHAOS」和「ADAPT & EXPERIMENT」。重大變更是改進願景和方法的機會。
- **PO 在重新規劃中的角色：** **Product Owner (PO)** 在管理重大變更的影響方面至關重要。他們可以「在發生重大變更時協助中途開發重新規劃或修正方向」。這包括重新評估優先順序、調整 backlog，並確保與整體專案目標一致。
- **Vibe CEO 的策略監督：** 作為「Vibe CEO」，您的角色是維持「STRATEGIC_OVERSIGHT」。當出現重大變更時，您將指導必要的轉向，確保專案與您的單一願景保持一致。
- **視需要重新接洽代理人：** 如果變更顯著改變了專案的範圍或基礎，請毫不猶豫地重新接洽早期階段的代理人（例如，Analyst 重新評估市場契合度，PM 修訂 PRDs，Architect 評估技術影響）。

## IDE 與 UI 使用 - 一般建議

BMAD 方法可以透過不同的介面進行協調，通常是 Web UI 用於較高層級的規劃，IDE 用於開發和技術任務。

### 概念與規劃階段

- **介面：** 通常最好透過 Web UI（利用帶有其打包資產和 [`agent-prompt.txt`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:289) 的 **Web Agent Orchestrator**）或專用的專案管理工具進行管理，其中 orchestrator 代理人可以指導流程。
- **涉及的代理人：**
  - **Analyst：** 腦力激盪、研究和初始 project brief 建立。
  - **PM (Product Manager)：** PRD 開發、epic 和高層級 story 定義。
- **活動：** 定義願景、初始需求收集、市場分析、高層級規劃。[`web-bmad-orchestrator-agent.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:293) 及其組態可能支援這些活動。

### 技術設計、文件管理與實施階段

- **介面：** 主要在整合開發環境 (IDE) 中，利用專業代理人（獨立的或透過以 [`ide-bmad-orchestrator-cfg.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:297) 設定的 **IDE Agent Orchestrator**）。
- **涉及的代理人：**
  - **Architect / Design Architect (UI)：** 詳細的技術設計和規格。
  - **POSM Agent：** 持續的文件管理和組織。
  - **PO (Product Owner) / SM (Scrum Master)：** 詳細的 story 產生、backlog 完善，通常直接在 IDE 或與其整合的工具中進行。
  - **Developer Agents：** stories 的程式碼實作，直接在 IDE 中處理程式碼庫。
- **活動：** 詳細架構、前端/後端設計、程式碼開發、測試，利用 IDE tasks（請參閱「利用 IDE TASKS 提升效率」），使用像 [`ide-bmad-orchestrator-cfg.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:303) 這樣的組態。

### BMAD 方法檔案

理解關鍵檔案有助於導覽和自訂 BMAD 流程：

- **知識與組態：**
  - [`bmad-agent/data/bmad-kb.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:310)：此中央知識庫。
  - [`ide-bmad-orchestrator-cfg.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:311)：IDE developer agents 的組態。
  - [`ide-bmad-orchestrator.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:312)：IDE orchestrator 代理人的定義。
  - [`web-bmad-orchestrator-agent-cfg.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:313)：web orchestrator 代理人的組態。
  - [`web-bmad-orchestrator-agent.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:314)：web orchestrator 代理人的定義。
- **Task 定義：**
  - [`bmad-agent/tasks/`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:316) 或 [`bmad-agent/checklists/`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:316) 中的檔案（例如 [`checklist-run-task.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:316)）：用於特定動作的可重複使用提示，也供代理人使用以保持代理人 persona 檔案精簡。
- **代理人 Personas 與 Templates：**
  - [`bmad-agent/personas/`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:318) 中的檔案：定義不同代理人的核心行為。
  - [`bmad-agent/templates/`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:319) 中的檔案：Project Briefs, PRDs 等文件的標準格式，代理人將使用這些格式來填入這些文件的實例。
- **專案產出物（Outputs - 位置因專案設定而異）：**
  - Project Briefs
  - Product Requirements Documents (PRDs)
  - UX/UI Specifications
  - Architecture Documents
  - 程式碼庫及相關開發檔案。

## 利用 IDE TASKS 提升效率

### IDE TASKS 的目的

- **減少代理人臃腫：** 避免向主要的 IDE 代理人模式（Dev Agent, SM Agent）甚至 Orchestrator 的基本提示中添加大量不常用的指令。保持代理人精簡，這對於有自訂代理人複雜度/數量限制的 IDEs 有益。
- **隨選功能：** 指示一個活躍的 IDE 代理人（獨立的或 IDE Orchestrator 中體現的 persona）執行一個 task，方法是提供相關 task 檔案的內容（例如，來自 [`bmad-agent/tasks/checklist-run-task.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:332)）作為提示，或者如果代理人被設定為可以找到它（如同 IDE Orchestrator），則參考它。
- **多功能性：** 任何能力足夠的代理人都可以被要求執行一個 task。Tasks 可以處理特定功能，例如執行 checklists、建立 stories、分割文件、索引函式庫等。它們是獨立的指令集。

### TASK 功能範例

**概念：** 將 tasks 視為專業的、可呼叫的迷你代理人或隨選指令集，主要的 IDE 代理人或 Orchestrator（當體現 persona 時）可以調用它們，從而保持主要代理人定義的精簡。它們對於不常執行的操作特別有用。[`docs/instruction.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:337) 檔案提供了有關 task 設定和使用的更多詳細資訊。

以下是一些由 [`bmad-agent/tasks/`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:339) 中的 tasks 提供的功能範例：

- **[`create-prd.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:341)：** 指導產生 Product Requirements Document。
- **[`create-next-story-task.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:342)：** 協助定義和建立下一個用於開發的 user story。
- **[`create-architecture.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:343)：** 協助概述專案的技術架構。
- **[`create-frontend-architecture.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:344)：** 特別專注於設計前端架構。
- **[`create-uxui-spec.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:345)：** 協助建立 UX/UI Specification 文件。
- **[`create-ai-frontend-prompt.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:346)：** 協助草擬用於 AI 服務以產生 UI/前端元素的提示。
- **[`doc-sharding-task.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:347)：** 提供將大型文件分解為較小、易於管理部分的流程。
- **[`library-indexing-task.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:348)：** 協助建立程式碼庫的索引或概覽。
- **[`checklist-run-task.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:349)：** 執行預定義的 checklist（可能使用 [`checklist-mappings.yml`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:349)）。
- **[`correct-course.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:350)：** 為專案需要調整方向時提供指引或步驟。
- **[`create-deep-research-prompt.md`](BMAD-METHOD-Chinese/bmad-agent/data/bmad-kb.md:351)：** 協助制定用於對某一主題進行深入研究的提示。

這些 tasks 允許代理人透過遵循每個 task 檔案中的詳細指令來執行複雜的多步驟操作，通常視需要利用 templates 和 checklists。
