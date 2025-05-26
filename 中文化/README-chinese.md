# BMAD-Method（Breakthrough Method of Agile (ai-driven) Development）

目前版本：Bmad Agent「V3 Release」

BMad Agent 全流程的 web agent 輸出展示可見於 [Demos](./demos/readme.md)——如果你想閱讀一份非常非常非常長的我與多重人格 BMad Agent 對話產生 demo 內容的逐字稿，可以在這裡閱讀 [full transcript](https://gemini.google.com/share/41fb640b63b0)。

## Web 快速入門專案設定（推薦）

Orchestrator Uber BMad Agent 一應俱全——已經 [預先建置](./web-build-sample/agent-prompt.txt)！只需將其複製到 Gemini Gem 或 custom GPT 作為指令，並依下圖將 web-build-sample 資料夾中的其他檔案附加給 agent：

![image info](./docs/images/gem-setup.png)

如果你不確定在 Web Agent 中該怎麼做，可以嘗試 /help 取得指令列表，並用 /agents 查看 BMad 能扮演哪些 personas。

若你打算在專案中使用 IDE Agents，複製此 repo 後，可以直接將 bmad-agent 資料夾複製到你的專案中——這是最簡單的方式。你也可以在此 repo 根目錄下用 nodeJS 執行指令來建置並打包資產，輕鬆打造一個能處理所有敏捷流程（從構思到 ready to develop）的超強 Web Agent（推薦）。

如果你想直接開始，這裡有 [IDE、WEB 與 Task 設定與使用說明](./docs/instruction.md)。

## IDE 專案快速入門

使用最新版 BMad Agents 進行 BMad Method 非常簡單——只需將 `bmad-agent` 資料夾複製到你的專案即可。前一版存在的 dedicated dev 與 sm 仍然可用，位於 `bmad-agent/personas` 資料夾，副檔名為 .ide.md。將內容複製貼上到你 IDE 的自訂 agent mode 設定方式即可。dev 與 sm 都預設將 architecture 與 prd 產物放在 (project-root)/docs，stories 會從 (project-root)/docs/stories 產生與開發。這會是預設行為，但很快會有 config 覆寫功能。

其他 agent 用法（包含 dev 與 sm）可設定 [ide orchestrator](./bmad-agent/ide-bmad-orchestrator.md)——你可以要求 orchestrator bmad 變身成任何你已[設定](./bmad-agent/ide-bmad-orchestrator-cfg.md)的 agent。

[一般 IDE Custom Mode 設定說明](./docs/ide-setup.md)。

## 推進 AI 驅動開發

歡迎體驗最新、最先進且易於使用的 Web 與 IDE Agent 敏捷流程！這個新版本稱為 BMad Agent，是在 [legacy V2](./legacy-archive/V2/) 基礎上大幅進化，帶來更精緻且完整的 agents、templates、checklists、tasks——還有強大的 BMad Orchestrator 與 Knowledge Base agent，能掌握方法論每個面向，變身成任何 agent，甚至能在單一龐大 web context 中同時處理多項任務。

## 有哪些新功能？

所有 IDE Agents 現在都優化在 6K 字元以內，因此能符合 windsurf 的檔案限制。

本方法現在有一個 uber Orchestrator，稱為 BMAD——這個 agent 能將你的 web 或 ide 使用體驗提升到新層次——它能變身成你想要合作的特定 agent！這讓 Web 使用變得非常簡單易上手。在 IDE 中，你也不必再設定那麼多不同的 agents（除非你想這麼做！）

文件與產物生成有了大幅提升，agents 現在能真正協助你打造最佳計畫。已導入進階 LLM 提示技巧，協助你與 agents 產出前所未見的精準產物。此外，agents 現在可設定其權限——你可以接受預設值，或自訂哪些 personas 能執行哪些任務。如果你認為 PO 應該產生 PRD、Scrum Master 應該負責校正方向——現在都做得到！**用 BMad 的方式定義敏捷——或用你的方式！**

雖然功能強大——你可以直接用本 repo 推薦的預設設定開始，基本上照著說明使用 agents 即可。詳細設定與使用方式請參見 [Instructions](./docs/instruction.md)。

## 什麼是 BMad Method？

BMad Method 是一種革命性方法，將「vibe coding」提升到進階專案規劃層次，確保你的 developer agents 能在明確指引下啟動並完成進階專案。它提供一個結構化且彈性的框架，讓你能用專業 AI agents 團隊規劃、執行與管理軟體專案。

這個方法與工具遠不只是任務執行器——它是一套精煉工具，協助你激發最佳創意、明確定義目標、並付諸實踐！從構思、PRD 撰寫到技術決策——都能在進階 LLM 指引下完成。

本方法設計原則上不依賴特定工具，agent 指令與流程可彈性適配各種 AI 平台與 IDE。

## Agile Agents

Agents 可以直接設計為自包含，直接放進 ide 的 agent config，也可以設計為 orchestrating agent 可變身的可程式化實體。

### Web Agents

Gemini 2.5 或 Open AI customGPTs 可透過執行 node build script 產生 build 資料夾的輸出。這份輸出就是建立 orchestrator web agent 的完整套件。

詳見 [Web Orchestration 設定與使用說明](./docs/instruction.md#setting-up-web-agent-orchestrator)

### IDE Agents

有專屬自包含 agent（stand alone），也有 IDE 版 orchestrator。standalone 包含：

- [Dev IDE Agent](./bmad-agent/personas/dev.ide.md)
- [Story Generating SM Agent](./bmad-agent/personas/sm.ide.md)

若要使用其他 agents，可直接從該資料夾取用——但有些會超過 Windsurf 限制，且數量眾多。建議用一次性任務，或更推薦——使用 IDE Orchestrator Agent。詳見 [IDE Orchestrator 設定與使用說明](./docs/instruction.md#ide-agent-setup-and-usage)。

## Tasks

位於 `bmad-agent/tasks/`，這些自包含指令集讓 IDE agents 或 orchestrators 設定的 agents 執行特定工作。也可作為一次性指令，直接在 ide 的 vanilla agent 參照該 task 並請 agent 執行。

**目的：**

- **減少 Agent 負擔：** 避免將不常用指令加入主要 agent。
- **隨需功能：** 只要提供 task 檔案內容，任何有能力的 IDE agent 都能執行。
- **多元用途：** 處理如執行 checklists、產生 stories、文件分片、索引函式庫等特定功能。

你可以把 tasks 想成可由主要 IDE agents 呼叫的專業小型 agents。

## End Matter

有興趣改進 BMAD Method？請參閱 [contributing guidelines](docs/CONTRIBUTING.md)。

感謝你的使用，祝順利——BMad！
[License](./docs/LICENSE)
