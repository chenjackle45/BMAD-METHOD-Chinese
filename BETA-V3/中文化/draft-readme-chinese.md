# The BMAD-Method (Breakthrough Method of Agile (ai-driven) Development) - BETA-V3

如果你想直接開始，請參閱 [安裝說明](./instruction.md)，適用於 IDE、WEB 及任務設定。

## BETA-V3：推進 AI 驅動開發

歡迎使用 BMAD Method 的 **BETA-V3**！本版本是在 V2 基礎上重大進化，導入更精緻且完整的代理、範本與流程。`BETA-V3` 資料夾包含最新實作，旨在提升協作、文件化，以及以 AI 進行專案開發的整體效率。

`LEGACY-V1` 與 `CURRENT-V2` 資料夾僅供歷史參考，**BETA-V3** 才是持續開發的重點，也是 BMAD Method 最先進的版本。

## 自訂模式與歡迎貢獻

BMAD 社群的回饋極為寶貴！我們鼓勵你透過 Pull Request 貢獻自訂代理模式、新範本或針對 `BETA-V3` 系統的檢查清單增強。不論是 Web UI（如 Gemini Gems/Custom GPTs）或 IDE 自訂模式，你的貢獻都能擴展我們 AI 團隊的能力。

`BETA-V3` 的自訂代理持續遵循 LLM 提示最佳實踐（如明確角色、指令與情境），確保能在各種 AI 平台上有效運作。

## 🔥 示範導覽 🔥

雖然專屬的 BETA-V3 影片導覽尚在規劃中，互動式、分階段代理協作的原則已可在 [V2 影片導覽](https://youtu.be/p0barbrWgQA?si=PD1RyWNVDpdF3QJf) 中充分展現。根目錄下的 [`V2-FULL-DEMO-WALKTHROUGH`](../V2-FULL-DEMO-WALKTHROUGH/demo.md) 資料夾展示了 BMAD Method 逐步、人工參與的強大流程。BETA-V3 進一步強化了這種互動性與文件連結。

## BETA-V3 有哪些新內容？

BETA-V3 引入多項關鍵強化，讓你的 AI 驅動開發流程更順暢：

- **代理角色與階段強化：** 核心代理（Analyst、PM、Architect）有更明確的運作階段、輸入與輸出，轉換更流暢。
- **新增專業代理：**
  - **Design Architect：** 專為有使用者介面的專案設計，分別處理 UI/UX 規格與前端技術架構。
  - **Technical POSM (Product Owner & Scrum Master)：** 統一代理，具備關鍵新功能：
    - **Master Checklist Phase：** 依據完整檢查清單（`po-master-checklist.txt`）驗證所有專案文件。
    - **Librarian Phase：** 將大型文件拆解為細緻、具索引（`docs/index.md`）的文件生態系，存於專案 `docs/` 資料夾，優化 AI 代理存取與人工瀏覽。
    - **Story Creator Phase：** 自動根據細緻文件產生詳盡、開發者可用的 story 檔案。
- **完整且更新的範本：** 新增與修訂範本支援擴展後的代理功能，包括：
  - `architecture-tmpl.txt`（系統架構用）
  - `front-end-spec-tmpl.txt`（UI/UX 規格用）
  - `front-end-architecture-tmpl.txt`（前端技術架構用）
  - `story-tmpl.txt`（開發者 story 用）
  - 其他範本位於 `BETA-V3/gems-and-gpts/templates/`。
- **詳細檢查清單：** 新增與更新的檢查清單確保各階段品質與完整性：
  - `pm-checklist.txt`
  - `architect-checklist.txt`
  - `frontend-architecture-checklist.txt`
  - `po-master-checklist.txt`（POSM 使用）
  - 皆位於 `BETA-V3/gems-and-gpts/checklists/`。
- **精簡流程與文件導向：** 更明確、反覆的流程涵蓋所有代理，強調建立並維護健全、細緻且具索引的文件結構以支援開發。
- **明確代理交接：** 更清楚說明每個代理的產出及後續代理所需的輸入。
- **多模式代理：** 持續推行代理以不同模式或階段執行特定任務的理念。
- **IDE 與 Web UI 一致性：** 代理指令與範本設計同時適用於 Web UI（見 `BETA-V3/gems-and-gpts/`）與 IDE 自訂模式（IDE 設定指引見 `BETA-V3/agents/instructions.md`，路徑待確認或建立）。

## 指導原則

- **無需規則（彈性）：** BMad Method 代理設計為自足或參照專案文件，最小化對專有 AI 平台規則系統的依賴，促進彈性並避免平台綁定。
- **IDE 整合：** 為獲得最佳體驗，特別是檔案系統操作（如 POSM Librarian 階段）與程式開發，建議使用支援自訂代理的 IDE（如 Cursor、RooCode）。自訂模式設定說明見 `BETA-V3/agents/instructions.md`（路徑待確認/建立）。
- **反覆與協作：** 方法強調逐步、互動式流程，代理與使用者協作，並在關鍵決策點暫停以取得輸入。

## 什麼是 BMad Method？

BMad Method 是一種革命性方法，將「vibe coding」提升為「Vibe CEOing」。它提供結構化且具彈性的框架，讓你能以專業 AI 代理團隊規劃、執行與管理軟體專案。BETA-V3 是最新進展，讓用戶能從構想到可實作的 developer story，藉由 AI 更快、更省、更有效地完成建置。

本方法原則上不依賴特定工具，代理指令與流程可適應各種 AI 平台與 IDE。

歡迎加入 [社群討論區](https://github.com/bmadcode/BMAD-METHOD/discussions) 共同貢獻與發展這些理念。

## BETA-V3 代理總覽

`BETA-V3` 系統具備精煉的專業 AI 代理團隊：

### 0. BMAD Method Advisor (`0-bmad.md`)

BMAD Method 的主要導覽，說明代理角色、流程與最佳實踐。

### 1. Analyst (`1-analyst.md`)

新想法或不明確構想的起點。

- **階段：** 腦力激盪、深度研究（廣泛市場/可行性）、專案簡報。
- **主要產出：** 提供 PM 參考的 **Project Brief**。

### 2. Product Manager (PM) (`2-pm.md`)

將想法/簡報轉化為詳細產品規劃。

- **階段：** 深度研究（聚焦產品策略驗證）、PRD 產生（含 Epics、User Stories、技術假設）、產品顧問。
- **主要產出：** 提供 Architect 與 Design Architect 的完整 **Product Requirements Document (PRD)**。

### 3. Architect (`3-architect.md`)

定義系統整體技術藍圖。

- **階段：** 深度研究提示產生（針對技術未知）、架構建立（定義技術棧、資料模型、服務）、Master Architect 諮詢（持續指導）。
- **主要產出：** **Technical Architecture Document**。

### 4. Design Architect (`4-design-architect.md`)

專責有使用者介面專案的 UI/UX 與前端技術策略。

- **模式：** UI/UX 規格（定義使用者體驗、流程、視覺指引）、前端架構（定義前端技術棧、元件、狀態管理）、AI 前端生成提示（為 AI 產碼器設計提示）。
- **主要產出：** **UI/UX Specification Document**（依 `front-end-spec-tmpl.txt`）與 **Frontend Architecture Document**（依 `front-end-architecture-tmpl.txt`）。

### 5. Technical POSM (`5-posm.md`)

確保文件品質並為開發做準備。

- **階段：**
  - **Master Checklist Phase：** 依 `po-master-checklist.txt` 驗證所有專案文件。產出：**Checklist Report**。
  - **Librarian Phase：** 將大型文件拆解為 `docs/` 內的細緻檔案，並建立/維護 `docs/index.md`。產出：**有組織的細緻文件**。
  - **Story Creator Phase：** 根據細緻文件與 PRD 自動產生開發用 **story 檔案**。
- **關鍵作用：** 文件完整性與為開發者準備可執行任務。

### 6. Developer Agents（如 `X-coder.md`、`Y-code-reviewer.md`）

（這些代理的專屬提示位於 `BETA-V3/agents/` 或 `BETA-V3/gems-and-gpts/`）
根據 POSM 的 story 實作功能，並遵循既定架構與程式標準。

## BETA-V3 步驟流程（典型流程）

1.  **Analyst (`1-analyst.md`)：**（可選）腦力激盪、研究 → **Project Brief**。
2.  **PM (`2-pm.md`)：** Project Brief/idea → **PRD**（Epics、Stories）。如有 UI 推薦 Design Architect。
3.  **Architect (`3-architect.md`)：** PRD → **System Architecture Document**。
4.  **Design Architect (`4-design-architect.md`)：**（如有 UI）PRD、System Arch → **UI/UX Spec**，再到 **Frontend Architecture Doc**。可選 AI 前端生成提示。
5.  **POSM (`5-posm.md`) - Master Checklist Phase：** 驗證所有文件 → **Master Checklist Report**。
6.  _(使用者/其他代理依 POSM 報告對原始文件進行修正)_。
7.  **POSM (`5-posm.md`) - Librarian Phase：** 處理更新後文件 → **細緻 `docs/` 檔案與 `docs/index.md`**。
8.  **POSM (`5-posm.md`) - Story Creator Phase：** 細緻文件與 PRD → **Developer Story Files**。
9.  **Developer Agents：** 實作 stories。
10. **持續諮詢：** Architect（Master Architect Advisory）與 PM（Product Advisor）持續支援。

_此為指引，BMAD 方法為反覆進行。_

## 範本與工具資訊（BETA-V3）

- **範本：** 代理產出核心範本（Project Brief、PRD、Architecture、Frontend Specs、Stories 等）位於 `BETA-V3/gems-and-gpts/templates/`。
- **檢查清單：** 各階段詳細檢查清單位於 `BETA-V3/gems-and-gpts/checklists/`。
- **Web UI（Gems/GPTs）：** 針對 Web UI（Gemini Gems、Custom GPTs）最佳化的代理指令位於 `BETA-V3/gems-and-gpts/`。請附上 `templates` 資料夾內的範本。詳見 `BETA-V3/gems-and-gpts/instruction.md`。
- **IDE 自訂模式：** IDE 代理設定指引見 `BETA-V3/agents/instructions.md`（此路徑僅為範例，實際 IDE 提示可能直接在 `BETA-V3/gems-and-gpts/` 或專屬 `BETA-V3/agents/` 資料夾內，如有針對 IDE 版本建立）。

## 授權

[License](./LICENSE)（假設授權條款位於根目錄並適用）

## 貢獻

有興趣改進 BMAD Method BETA-V3？請參閱我們的 [貢獻指南](./CONTRIBUTING.md)。（假設貢獻指南位於根目錄）
