[English Version](./README.md)

# BMAD 方法 (突破性敏捷開發 AI 驅動方法)

## 重大更新：V2 最終版本

BMAD 方法隨著我們的 V2 (測試版) 發布有了重大轉變！先前的實作版本（仍可在 `LEGACY-V1` 資料夾中找到）已被 `CURRENT-V2` 資料夾中大幅改進的工作流程和代理系統所取代。

注意：V1 將很快被移除，V2 將成為未來主要的模式。

## 自訂模式與歡迎貢獻

感謝社群提供許多優秀的建議和想法 - 歡迎您提交 PR，建議任何適用於 Gems/GPTs 或 IDE（或兩者）的自訂代理模式，協助擴展我們 AI 助手團隊的陣容 :)

這些自訂代理遵循 [格式規則的最佳實踐](https://docs.windsurf.com/windsurf/memories#best-practices)，以保持對 LLMs 的最佳化（這些指引與官方的 sonnet、openAI 和 Gemini 提示指南一致），並已證實可在 Cursor、Windsurf、VSCode（+ Client、Roo、CoPilot）中良好運作。

## 🔥 完整展示教學 🔥

**觀看實際操作！** [影片展示](https://youtu.be/p0barbrWgQA?si=PD1RyWNVDpdF3QJf) - [`V2-FULL-DEMO-WALKTHROUGH`](./V2-FULL-DEMO-WALKTHROUGH/demo.md) 資料夾包含了 BMAD 方法 V2 的完整端對端展示。這不只是文件—它證明了 BMAD 方法在 Vibe-CEO 方面的卓越效果和強大功能！

此展示包含：

- 每個階段的 **完整 Gemini 聊天記錄**
- 代理之間流通的 **所有輸出成品**
- 互動過程的 **詳細評註**

這個展示特別之處在於展現代理如何在**互動階段**運作，在關鍵決策點暫停等待人工輸入，而不是一次性產出大量文件。沒有其他 AI 開發方法能提供這種協作工作流程的紀錄，讓代理能夠與您一步一步合作。

[探索展示](./V2-FULL-DEMO-WALKTHROUGH/)，看看這如何改變從構想到實施就緒的代理故事和任務的軟體開發過程。

## V2 新功能

- **最佳化的代理提示**：完全修訂的代理提示以獲得更好的輸出
- **標準化模板**：全面的模板集以保持文件創建的一致性
- **精簡的工作流程**：從構想到部署的更清晰過程
- **改進的敏捷整合**：對敏捷方法論的更好支持
- **代理與寶石代理的區分**：V2 擁有特定的 [寶石](#custom-gems-and-gpts)（嵌入模板的代理）與 IDE 代理達成平衡。
- **全面的檢查清單**：針對 PM、架構師和 PO 角色的新詳細檢查清單，以確保每個階段的質量和完整性！
- **多模式代理**：每個代理現在在不同的模式下運作，這些模式針對不同的專案階段或複雜性水平量身定制 - 允許靈活性以匹配您的專案需求和知識水平
- **減少冗餘**：V2 代理和寶石使用一致的 XML 標記格式，這些格式經過優化以適應 LLM，使其更高效且更易於維護
- **開發-SM 對等**：SM 代理生成的故事與開發代理自行加載的內容保持對等，減少重複並確保順利交接。如果不使用開發代理自訂模式，請確保您的 IDE 代理規則包含開發代理的所有信息以保持兼容性。例如，開發代理知道加載 docs/coding-standards 和 project-structure - 因此這不應該在生成的故事中重複（但可以鏈接）。

## 無需規則！

BMAD 方法的最大優勢之一是使用自訂代理時不需要自訂規則。開發代理和其他角色被配置為在編碼時自動引用標準文件。這提供了兩個主要好處：

1. **無平台鎖定**：在任何 AI 系統上工作，而不必依賴專有的規則格式
2. **最大靈活性**：如果您更喜歡那種方法，仍然可以與基於規則的系統（如 Claude 或 Cursor）兼容

這種靈活性使您可以選擇最適合您的工作流程的實施方式，同時在整個專案中保持一致的質量。

## IDE 代理整合

BMAD 方法現在包括詳細的 [自訂代理模式設置說明](./CURRENT-V2/agents/instructions.md) 在各種 IDE 中。對於完整代理系統的最直接轉換，特別是對於關鍵的 SM 和 Dev 代理角色，建議使用以下工具：

- **RooCode**：出色的自訂模式支持，具有強大的文件權限控制和模式切換
- **Cursor**：強大的自訂代理功能，具有先進的上下文管理

這兩者都提供了實施 BMAD 工作流程的最佳體驗，儘管該方法可以適應任何 IDE。請參閱完整的 [說明文件](./CURRENT-V2/agents/instructions.md)，了解如何配置每個支持的 IDE。

## BMAD 方法是什麼？

BMAD 方法是一種革命性的方法，將「氛圍編碼」提升到一個新水平—我稱之為「Vibe CEOing」。與純粹的氛圍編碼（用於快速原型設計）的自發性不同，該方法幫助您計劃、執行並保持專案正軌。利用 AI 驅動的過程，加速和增強從構思、市場契合到代理代碼實施的整個產品開發生命周期，更快、更便宜、更輕鬆地構建。

此 V2 版本吸收了 V1 用戶的寶貴反饋，並受到 Claude 記憶庫和 Taskmaster 項目等項目的啟發。然而，BMAD 方法更進一步，為那些希望從概念到完成徹底定義和開發專案的人提供了一個全面的框架。

這種全面的、逐步的方法將產品想法轉變為完全實施的應用程序，通過：

1. 將開發過程結構化為不同的 AI 角色基礎階段
2. 在每個階段生成詳細的產物
3. 使用順序工作流程跟踪進度
4. 使 AI 能夠根據生成的規範編寫完整的應用程序

該方法與工具無關，工作流程內置於角色提示中，適用於任何代理編碼工具集、IDE 或平台。

加入 [社區討論論壇](https://github.com/bmadcode/BMAD-METHOD/discussions)，幫助貢獻和發展這些想法。

### 影片教學

- 過時的工作流程在 [BMAD Code YouTube 頻道的第 1 和第 2 部分](https://youtu.be/JbhiLUY_V2U) 中解釋
- V2 工作流程的新教程將很快推出 - 包括完整的端到端專案構建！

## 即將推出（大約）

- 一個簡單和高級專案產物文件的完整輸出，執行代理以構建專案構建所需的所有產物，並附有提示範例，說明如何從代理輸出中獲取結果。
- 探索利用 MCP 的可能性。

## 工作流程視覺圖

[查看圖表](./workflow-diagram.md)

## 自訂代理概述

### 分析師（商業分析師 (BA)、研究助理 (RA)、頭腦風暴教練）

分析師代理是一個多功能的 BMAD 方法切入點，通過創意構思技術、市場分析或結構化需求收集，運行三種不同模式，幫助將初步想法或模糊概念轉變為明確的專案簡報。該代理非常適合需要幫助提煉其願景的用戶，然後再進入詳細的產品定義。

#### 頭腦風暴教練模式

此模式使用 SCAMPER 和「如果」場景等經驗證的頭腦風暴技術來擴展可能性。該代理通過結構化的構思框架指導用戶，鼓勵發散性思維，並幫助挑戰可能限制創造力的假設。

#### 深入研究模式

此模式生成全面的研究提示，以探索市場需求、競爭對手和目標用戶。它專注於深入研究市場狀況、行業趨勢、競爭格局和用戶需求，以指導專案方向。

#### 專案簡報模式

此模式協作構建一個結構化的簡報文件，並提供專門的 PM 代理交接提示部分，為下一階段提供戰略指導。它利用研究結果和用戶輸入創建一個明確的專案簡報，作為產品開發的基礎。

隨著 V2 最終版的發布，這種模式就像魔法一樣 - 作為您的合作夥伴教練和頭腦風暴大師，幫助您在願景的那顆種子中找到想法 - 特別是在使用 Gemini 2.5 pro 思維時。作為額外獎勵，您甚至可以將最終的專案簡報輸出作為另一輪頭腦風暴的基礎，以便在交給 PM 之前進一步完善（這是我發現的意外驚喜，結果非常好）。

### 產品經理 (PM)

產品經理代理擅長將高階專案簡報或初步想法轉變為全面的產品規範和可行的開發計劃。作為範圍精煉專家，PM 積極挑戰對 MVP 真正必要的功能的假設，尋找簡化的機會，同時確保與核心商業目標的完美對齊。

#### 模式 1：初步產品定義

在此模式中，PM 從頭開始創建 MVP 的核心產品定義文件。該代理生成三個關鍵產物：詳細的產品需求文檔 (PRD)，概述目標、功能和非功能需求；一組將工作分解為可獨立部署塊的史詩定義；以及捕捉關鍵技術決策的初始架構提示。在整個過程中，PM 參與多輪範圍精煉—首先是在初步範圍討論期間，然後是在草擬 PRD 之後，最後是在創建史詩之後—始終圍繞時間、成本和質量權衡框架進行對話。PM 還確定部署考慮因素和測試要求，確保每個史詩在邏輯上建立在先前的基礎上，史詩 1 包含所有必要的基礎設施設置。

當在 Google Gemini 2.5 Pro 思維中運行時，這種模式現在就像魔法一樣 - 它清楚地停止、確認和解釋每個選擇和描述，使過程變得輕鬆，並將指導您每一步。請享受最終的 V2 發布！

#### 模式 2：產品精煉與諮詢

在此模式下（當 PRD 已經存在並獲得批准時），PM 作為持續的產品顧問，提供對現有需求的澄清，促進產品隨著時間的推移而進行修改，評估擬議變更的影響，管理範圍調整，並在整個開發過程中保持一致、最新的產品文檔。

### 架構師

架構師代理是一位專家級解決方案/軟體架構師，運行三種不同模式以支持產品開發生命周期中的技術設計。憑藉對雲平台、架構、數據庫和程式語言的深刻理解，架構師將需求轉化為強大、可擴展的技術設計，這些設計經過優化以便 AI 代理實施。

#### 模式 1：深入研究提示生成

此模式創建全面的研究提示，以在做出架構決策之前調查技術選項、平台、服務和實施方法。架構師分析可用的專案上下文，確定研究差距，並構建詳細的提示，定義明確的目標、概述具體問題、請求比較分析，並建立決策評估框架。

**寶石模式獎勵**：寶石版本包括一個廣泛的示例研究提示模板，用於後端技術棧評估，演示如何構建全面的技術調查。該模板展示了如何定義研究目標、具體技術、評估維度、實施考慮因素和決策框架—為任何技術領域創建針對性研究提示提供藍圖。

#### 模式 2：架構創建

在此模式中，架構師根據 PRD、史詩和專案簡報設計並記錄完整的技術架構。該代理做出明確的技術決策（而不是開放式選項），解釋關鍵選擇背後的理由，並生成所有必要的技術產物，包括詳細的架構文檔、技術棧規範、專案結構、編碼標準、API 參考、數據模型、環境變數和測試策略—all optimized for implementation by AI agents.

當在 Google Gemini 2.5 Pro 思維中運行時，這種模式現在就像魔法一樣 - 它清楚地停止、確認和解釋每個選擇和描述，使過程變得輕鬆，並將指導您每一步。請享受最終的 V2 發布！

#### 模式 3：首席架構師諮詢

此模式在專案整個過程中提供持續的技術指導，解釋概念、建議對產物的更新，並管理技術方向的變更。架構師評估變更對整個專案的影響，建議最小干擾的調整方法，確定技術債務，並確保所有重要決策都得到妥善記錄。該代理在有助於清晰的情況下使用清晰的 Mermaid 圖表來直觀地表示系統結構和交互。

### 產品負責人 (PO)

產品負責人代理在開發開始之前，對整個 MVP 計劃進行驗證和質量保證檢查。使用全面的驗證檢查清單，PO 代理系統地審查所有專案產物，以確保它們完整、結構良好且正確對齊。PO 代理驗證專案設置和初始化步驟的正確性，驗證基礎設施和部署順序，確認所有外部依賴項得到妥善處理，劃分用戶與代理的責任，確保功能正確排序並管理依賴關係，驗證 PRD 中的所有 MVP 目標在史詩/故事中得到體現，並檢查所有技術需求是否得到滿足。該代理還評估風險管理策略、文檔完整性，並驗證後 MVP 考慮事項是否得到妥善處理。這一系統的驗證過程有助於及早發現潛在問題，確保開發階段的順利進行。

### 敏捷教練 (SM)

敏捷教練代理作為批准計劃與可執行開發任務之間的技術橋樑。作為專家級技術敏捷教練/高級工程師，該代理系統地識別基於依賴關係和前提條件的下一個合邏輯的故事，僅提取必要的技術上下文從參考文檔中創建自包含的故事文件。SM 在一個以過程為驅動的工作流程中運作，包括檢查前提狀態、收集相關需求和技術上下文、填充標準化故事模板、根據清單驗證故事完整性，並將故事標記為準備開發。該代理標記缺失或矛盾的信息為阻塞因素，專注於提供足夠的上下文而不過度指定實施細節，並確保每個故事包含具體的測試要求和驗收標準。

### PO-SM (寶石) 組合

PO-SM 寶石代理是一個雙模式專家，運行兩種不同的角色：

#### PO 模式（計劃驗證者）

在產品負責人模式下，該代理對整個 MVP 計劃進行全面驗證，然後再開始開發。該代理系統地通過詳細的檢查清單，驗證基礎實施邏輯、技術順序的可行性和 PRD 的對齊情況。它評估適當的初始化步驟、基礎設施排序、用戶與代理行動的適當性以及外部依賴項的管理。該代理做出明確的「繼續/停止」決定，要么批准計劃，要么提供具體的可操作反饋以解決不足之處。

IDE（代理資料夾）版本中的 PO 模式是主要代理，負責生成專案根目錄 `docs/_index.ts`，或在需要時更新它，因為隨著更多文檔的創建而需要更新 - 這是 V2 的一個很好的補充，將來的代理將有一個目錄來發現您為庫存 produced 的所有文檔，並從主庫中鏈接到這些文檔。這是代理知識的未來，關於如何在您的庫隨著時間的推移而增長以及未來的代理如何發展。

#### SM 模式（故事生成器）

計劃獲得批准後，該代理轉換為敏捷教練模式，在該模式下，它識別剩餘的故事，收集技術上下文，並生成遵循標準模板的全面故事文件。在此模式中，該代理僅從參考文檔中提取故事特定的信息，填充模板適當的細節，並以清晰的部分標題和邊界格式化每個故事。該代理可以生成所有剩餘故事的完整報告，這些故事根據依賴關係以合邏輯的順序組織。

### 開發代理 (Dev)

開發代理是一位專家級軟體開發人員，負責實施來自指定故事文件的需求。該代理專注於故事實施，同時遵循專案標準，按順序處理故事文件中定義的任務。開發代理嚴格遵守編碼標準和專案結構，而無需在每個故事中重複這些標準，根據專案測試策略實施測試，並通過更新故事狀態來報告進度。該代理以專注的自主性運作，僅在真正被模糊性阻塞時才會提出問題，並首先嘗試使用可用文檔解決問題。在完成所有任務並驗證測試通過後，開發代理將故事標記為待審核，並等待反饋然後再進行部署。

## 步驟流程

### 第 0 階段：構思（可選）

1. 如果您的想法模糊，請從 **商業分析師 (BA)** 代理開始
2. BA 將幫助分析市場狀況和競爭對手
3. 輸出為 **專案簡報**，用作產品經理的輸入

如果您不確定，請嘗試對 BA 進行如下簡單目標引出的頭腦風暴：

- 我有一個應用程式的想法，我想和你一起頭腦風暴，
- 它可以做到（X）
- 以便（y）
- 針對（Z）用戶

  **示例：** '我有一個應用程式的想法，我想和你一起頭腦風暴，它可以**幫助小企業主自動生成**專業的社交媒體內容，讓他們**無需雇用昂貴的營銷機構**就能保持一致的在線形象，適用於**時間緊迫的企業家**，他們知道社交媒體很重要，但沒有時間或技能來每天創建優質內容'

### 第 1 階段：產品定義

1. **產品經理 (PM)** 代理接受專案簡報或您的想法
2. PM 創建全面的產品需求文檔 (PRD)，並附上高級史詩/故事流程文檔。 （單獨的文件減少了未來代理的整體上下文 - 這是在 V2+ 版本中）
3. 草擬初始史詩分解

### 第 2 階段：技術設計

1. **架構師** 代理審查 PRD，並創建一個架構文檔，在 V2 中生成優化 LLM 上下文空間的細粒度產物！
2. 架構師確定技術依賴關係和參考文件
3. 可選：使用深入研究模式進行更深入的技術探索

### 第 3 階段：精煉和驗證

1. PM、架構師和敏捷教練協作精煉計劃
2. **產品負責人 (PO)** 驗證範圍、故事順序和 PRD 對齊
3. 最終文檔獲得批准並編入索引

### 第 4 階段：故事生成

1. **技術敏捷教練** 為開發人員生成每個故事（或在寶石/GPT 模式下生成所有剩餘故事）。輸出故事或故事是高度詳細的，包含代理所需的所有技術細節，以保持指導上下文的精簡 - 無需搜索所有較大文檔，避免不必要的信息膨脹其上下文。

### 第 5 階段：開發

1. **開發代理** 一次處理一個故事
2. 開發人員在實施之前創建故事草稿以供審核
3. 代碼和測試被提交，故事文件被更新

### 第 6 階段：審核和接受

1. 由新的開發代理或架構師進行代碼審核
2. 由用戶/質量保證進行功能審核
3. 獲得批准後，故事標記為完成

### 第 6.5 階段：重構（可選步驟）

1. 架構師或開發代理在新的內容中，或由用戶進行重構，並確保如果重構改變結構，則源樹圖和未來實施文檔會根據需要更新。
2. 在大多數故事之後，通過在之前的階段正確遵循流程，生成穩固的計劃和架構，這通常不是必需的 - 但即使是最好的計劃有時也會導致出現美觀重構的想法。此外，即使是最好的計劃，有時也會導致代理做出不理想的事情，這些事情最好進行修正。
3. 請記住，此階段重構的主要關鍵是確保所有內容首先正常運行，完整的應用程序和新功能通過測試和檢查，已提交並推送到遠程分支。只有在這種情況下，我才會進行重大（甚至輕微的重構）。如果我自己進行重構，我會更有信心，可能會在推送和標記故事完成之前進行。但對於代理，請始終鎖定您的工作更改，以便您有一個逃生舱來恢復！

### 第 7 階段：部署

1. 開發代理處理部署
2. 使用代碼命令進行基礎設施即代碼部署
3. 系統更新新功能

## 模板依賴

**重要：** 此系統中的代理（不是寶石）依賴於位於 `CURRENT-V2/docs/templates` 資料夾中的模板文件。這些模板應保持原名以確保正確功能，並放入您的專案 docs/templates 資料夾中，包括：

- `architecture.md`
- `story-template.md`
- `prd.md`
- `project-brief.md`
- 等等...（所有模板文件）

## 自訂寶石和 GPT（強烈建議）

**使用帶有基於網頁的 AI 介面的代理（強烈建議，節省大量金錢，更大的上下文窗口，深入研究是遊戲規則改變者）**

[寶石資料夾](./CURRENT-V2/gems-and-gpts/) 包含嵌入優化網頁使用說明的代理說明 - 為 [Gemini Gems](https://gemini.google.com/gems/create) 或 [OpenAIs custom GPT's](https://chatgpt.com/gpts/editor) 簡化了使用。使用自訂 GPT 和 Gemini 寶石，您將現在附加模板，而不是嵌入它們（這是 V1 系統的情況，修改起來不那麼容易）。這樣，當您修改來自模板資料夾的模板時，如果您想在網頁版本中更改它，只需將其另存為 .txt 擴展名並附加到自訂 GEM 或 GPT。

[詳細的設置和使用說明](./CURRENT-V2/gems-and-gpts/instruction.md) 也可在資料夾中找到。

## IDE 整合

該方法適用於任何 IDE 或 AI 助手，具體方法如下：

- **自訂模式**：適用於支持自訂模式的 IDE（Cursor、RooCode）
- **自訂寶石**：對於 Gemini，為每個代理創建一個自訂寶石
- **直接提示**：對於任何 AI 助手，直接使用代理提示

## 授權

[授權](./LICENSE)

## 貢獻

有興趣改善 BMAD 方法嗎？請參閱我們的 [貢獻指南](CONTRIBUTING.md)。
