[Read in English (閱讀英文版)](../README.md)

# BMAD 方法 (突破性的敏捷 (AI 驅動) 開發方法)

目前版本：V3 發行預覽版 "Bmad Agent"

BMad Agent 與完整工作流程的示範很快會在 [Demos](../demos/) 中提供。

## Web 快速入門專案設定 (建議)

Orchestrator Uber BMad Agent 包辦一切 - 已經 [預先建置](../web-build-sample/agent-prompt.txt)！只需將其複製到 Gemini Gem 或自訂 GPT 作為指示，並如下圖所示將資料夾中其餘檔案附加到 agent。

![圖片資訊](../docs/images/gem-setup.png)

如果您不確定在 Web Agent 中該做什麼 - 請嘗試輸入 `/help` 以取得指令列表，並輸入 `/agents` 查看 BMad 可以扮演哪些角色。

如果您打算在專案中使用 IDE Agent，複製 repo 後，您可以視情況將 `bmad-agent` 資料夾原封不動地複製到您的專案中 - 這是最簡單的方法。您也可以從此複製的 repo 根資料夾執行指令，搭配 NodeJS 來建置並打包您的資產，以便輕鬆編寫一個功能超強的 Web Agent 來處理從概念發想到準備開發的所有敏捷流程 (建議)。

因此，如果您想立即開始，這裡是 IDE、WEB 和 Task 設定的[設定與使用說明](./docs/instruction-chinese.md)。

## IDE 專案快速入門

開始使用最新版的 BMad Method 的 BMad Agent 非常簡單 - 您只需將 `bmad-agent` 資料夾複製到您的專案即可。先前版本中存在的專用 dev 和 sm 仍然可用，並且位於 `bmad-agent/personas` 資料夾中，副檔名為 `.ide.md`。將內容複製並貼到您特定 IDE 設定自訂 agent 模式的方法中。dev 和 sm 都設定為架構和 prd 成品位於 (專案根目錄)/docs，而 story 將從 (專案根目錄)/docs/stories 產生和開發。這將維持為預設值，但很快會提供設定覆寫功能。

對於所有其他 agent 用途 (包括 dev 和 sm)，您可以設定 [ide orchestrator](./bmad-agent/ide-bmad-orchestrator-chinese.md) - 您可以要求 orchestrator bmad 成為您已[設定](./bmad-agent/ide-bmad-orchestrator-cfg-chinese.md)的任何 agent。

## 推進 AI 驅動的開發

歡迎使用最新、最先進且易於使用的 Web 和 IDE Agent 敏捷工作流程版本！這個名為 BMad Agent 的新版本代表著重大的演進，它建立在[舊版 V2](../legacy-archive/V2/) 的基礎之上，但進行了大幅改進，引入了一套更精緻、更全面的 agent、範本、檢查清單、task - 以及令人驚豔的 BMad Orchestrator 和知識庫 agent 現已推出 - 它是該方法各個方面的專家，可以扮演任何 agent，甚至可以在單一龐大的 Web 情境中處理多個 task (如果需要)。

## 有哪些新功能？

所有 IDE Agent 現在都經過最佳化，字元數少於 6K，因此它們將適用於 windsurf 的檔案大小限制。

此方法現在有一個名為 BMAD 的 uber Orchestrator - 這個 agent 將把您的 Web 或 IDE 使用體驗提升到新的境界 - 這個 agent 可以變形並成為您想要使用的特定 agent！這使得 Web 使用變得超級簡單易用和設定。而在 IDE 中 - 如果您不想，就不必設定那麼多不同的 agent！

文件和成品的產生方式已大幅改進，agent 現在經過編寫，能真正協助您建立最佳的計畫。已納入並編寫了先進的 LLM 提示技巧，以協助您讓 agent 產生前所未見的驚人準確成品。此外，agent 現在可以設定其功能範圍 - 因此您可以接受預設值，或設定哪些角色可以執行哪些 task。如果您認為 PO 應該負責產生 PRD，而 Scrum Master 應該是您的方向修正者 - 現在一切皆有可能！**以 BMad 的方式定義敏捷 - 或以您的方式！**

雖然這非常強大 - 您可以從此 repo 中的預設建議設定開始，基本上按照所構想和將解釋的方式使用 agent。詳細的設定和用法在[說明](./docs/instruction-chinese.md)中概述。

## 什麼是 BMad Method？

BMad Method 是一種革命性的方法，它將「憑感覺編碼 (vibe coding)」提升到進階專案規劃的層次，以確保您的開發者 agent 能夠在非常明確的指導下啟動並完成進階專案。它提供了一個結構化且具彈性的框架，可使用一組專業的 AI agent 來規劃、執行和管理軟體專案。

這個方法和工具遠不止是一個 task 執行器 - 它是一個精密的工具，將協助您激發出最佳創意，定義您真正要建置的內容，並加以執行！從概念發想、PRD 建立到技術決策制定 - 這將協助您在先進 LLM 指導的強大功能下完成所有工作。

此方法原則上設計為工具中立，agent 指示和工作流程可適應各種 AI 平台和 IDE。

## 敏捷 Agent

Agent 的編寫方式有兩種：一種是直接獨立包含，可直接放入 IDE 中的 agent 設定；另一種是可設定為可編程實體，讓 orchestrating agent 可以扮演。

### Web Agent

Gemini 2.5 或 Open AI customGPTs 是透過執行 node 建置腳本來產生輸出到建置資料夾而建立的。此輸出是建立 orchestrator web agent 的完整套件。

請參閱詳細的[Web Orchestration 設定與使用說明](./docs/instruction-chinese.md#設定-web-agent-orchestrator)

### IDE Agent

有專用的獨立 agent，也有 IDE 版本的 orchestrator。對於獨立 agent，有：

- [Dev IDE Agent](./bmad-agent/personas/dev.ide-chinese.md)
- [Story 產生 SM Agent](./bmad-agent/personas/sm.ide-chinese.md)

如果您想使用其他 agent，可以使用該資料夾中的其他 agent - 但有些會超過 Windsurf 允許的大小 - 而且有很多 agent。因此建議使用一次性的 task - 或者更好的是 - 使用 IDE Orchestrator Agent。請參閱這些[IDE Orchestrator 的設定與使用說明](./docs/instruction-chinese.md#ide-agent-設定與使用)。

## Task

位於 `bmad-agent/tasks/`，這些獨立的指令集讓 IDE agent 或 orchestrator 設定的 agent 能夠執行特定的工作。這些也可以作為一次性指令，與 IDE 中的 vanilla agent 一起使用，只需參考 task 並要求 agent 執行即可。

**目的：**

- **減少 Agent 臃腫：** 避免將不常用的指令新增到主要 agent。
- **隨選功能：** 透過提供 task 檔案內容，指示任何有能力的 IDE agent 執行 task。
- **多功能性：** 處理特定功能，如執行檢查清單、建立 story、分割文件、索引程式庫等。

將 task 視為可由您的主要 IDE agent 呼叫的特製迷你 agent。

## 結語

有興趣改進 BMAD Method 嗎？請參閱[貢獻指南](../../docs/CONTRIBUTING.md)。

感謝您的使用並祝您愉快 - BMad！
[授權](../../docs/LICENSE)
