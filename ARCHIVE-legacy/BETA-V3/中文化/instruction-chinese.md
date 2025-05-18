# 指令

- [在 Gemini Gem 或 ChatGPT Custom GPT 中設定 Web 模式代理程式](#在-gemini-gem-或-chatgpt-custom-gpt-中設定-web-模式代理程式)
- [IDE 代理程式設定](#ide-代理程式設定)
- [任務設定與使用](#任務)

## 在 Gemini Gem 或 ChatGPT Custom GPT 中設定 Web 模式代理程式

若要設定 Web 模式代理程式，請參閱下表。其中概述了代理程式名稱、其描述的來源檔案，以及任何需要附加的檢查清單或範本檔案。

| 代理程式名稱       | 描述檔案路徑                                    | 附件檔案 (檢查清單/範本)                                                                                                                                                       |
| ------------------ | ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0-bmad             | `BETA-V3/web-agent-modes/0-bmad.md`             |                                                                                                                                                                                |
| 1-analyst          | `BETA-V3/web-agent-modes/1-analyst.md`          | `BETA-V3/templates/project-brief-tmpl.txt`                                                                                                                                     |
| 2-pm               | `BETA-V3/web-agent-modes/2-pm.md`               | `BETA-V3/checklists/pm-checklist.txt`, `BETA-V3/templates/prd-tmpl.txt`                                                                                                        |
| 3-architect        | `BETA-V3/web-agent-modes/3-architect.md`        | `BETA-V3/templates/architecture-tmpl.txt`, `BETA-V3/checklists/architect-checklist.txt`                                                                                        |
| 4-design-architect | `BETA-V3/web-agent-modes/4-design-architect.md` | `BETA-V3/templates/front-end-architecture-tmpl.txt`, `BETA-V3/templates/front-end-spec-tmpl.txt`, `BETA-V3/checklists/frontend-architecture-checklist.txt`                     |
| 5-posm             | `BETA-V3/web-agent-modes/5-posm.md`             | `BETA-V3/checklists/po-master-checklist.txt`, `BETA-V3/templates/story-tmpl.txt`, `BETA-V3/checklists/story-draft-checklist.txt`, `BETA-V3/checklists/story-dod-checklist.txt` |
| 6-rte              | `BETA-V3/web-agent-modes/6-rte.md`              | `BETA-V3/checklists/rte-checklist.md`                                                                                                                                          |

## IDE 代理程式設定

V3 中的 IDE 代理程式均已最佳化，總大小低於 6k，以相容於 Windsurf，並且通常更適合 IDE 使用！請確保您有一個 docs 資料夾，其中包含 templates/ 和 checklists/ 資料夾。

### Cursor

Cursor 目前僅支援最多 5 個自訂代理程式 - 因此對於 Cursor，強烈建議對那些可以在 Web 版本中使用的代理程式使用 Web 版本，並將 IDE 中的代理程式自訂模式設定保留給那些適合在 IDE 中使用的代理程式 - 至少包括 dev agent、sm agent。我可能只會設定這些，因為我喜歡為更專業的自訂開發人員保留空間。

V3 中引入了 Tasks，Workflows 也即將推出 - 這將很快允許更通用的 agile pro 代理程式處理目前由多個代理程式執行的多數前期工作。

#### 在 Cursor 中設定自訂模式

1.  **存取代理程式組態**：

    - 導覽至 Cursor 設定 > 功能 > 聊天與編輯器
    - 尋找「AI 規則」部分以設定所有代理程式的基本指南

2.  **建立自訂代理程式**：

    - 可以建立自訂代理程式並使用特定工具、模型和自訂提示進行組態
    - Cursor 允許透過 GUI 介面建立自訂代理程式
    - 請參閱 [Cursor 自訂模式文件](https://docs.cursor.com/chat/custom-modes#custom-modes)

3.  **組態 BMAD 方法代理程式**：
    - 在您的工作流程中為每個代理程式定義特定角色 (Analyst、PM、Architect、PO/SM 等)
    - 指定每個代理程式可以使用的工具 (Cursor 原生工具和 MCP)
    - 設定自訂提示，定義每個代理程式應如何運作
    - 根據其角色控制每個代理程式使用的模型
    - 設定他們可以和不可以 YOLO 的內容

### Windsurf

所有 V3 代理程式均已最佳化，字元限制低於 6K，現在非常適合 Windsurf 使用！

#### 在 Windsurf 中設定自訂模式

1.  **存取代理程式組態**：

    - 按一下右下角的「Windsurf - 設定」按鈕
    - 透過設定面板中的按鈕或右上角的個人資料下拉式選單存取進階設定

2.  **組態自訂規則**：

    - 為 Cascade (Windsurf 的代理程式聊天機器人) 定義自訂 AI 規則
    - 指定代理程式應以特定方式回應、使用特定框架或遵循特定 API

3.  **使用 Flows**：

    - Flows 結合了代理程式和 Copilots，形成一個全面的工作流程
    - Windsurf 編輯器專為可以獨立處理複雜任務的 AI 代理程式而設計
    - 使用模型內容協定 (MCP) 來擴展代理程式功能

4.  **BMAD 方法實作**：
    - 為 BMAD 工作流程中的每個角色建立自訂代理程式
    - 為每個代理程式組態適當的權限和功能
    - 利用 Windsurf 的代理程式功能來維持工作流程的連續性

### RooCode

#### 在 RooCode 中設定自訂代理程式

1.  **自訂模式組態**：

    - 透過組態檔案建立量身打造的 AI 行為
    - 每個自訂模式都可以有特定的提示、檔案限制和自動核准設定

2.  **建立 BMAD 方法代理程式**：

    - 為每個 BMAD 角色 (Analyst、PM、Architect、PO/SM、Dev、Documentation 等) 建立不同的模式
    - 使用針對其角色量身打造的提示自訂每個模式
    - 組態適合每個角色的檔案限制 (例如，Architect 和 PM 模式可以編輯 markdown 檔案)
    - 設定直接模式切換，以便代理程式在需要時可以請求切換到其他模式

3.  **模型組態**：

    - 為每個模式組態不同的模型 (例如，用於架構的進階模型與用於日常編碼任務的較便宜模型)
    - RooCode 支援多個 API 供應商，包括 OpenRouter、Anthropic、OpenAI、Google Gemini、AWS Bedrock、Azure 和本地模型

4.  **使用情況追蹤**：
    - 監控每個工作階段的 token 和成本使用情況
    - 根據任務的複雜性最佳化模型選擇

### Cline

#### 在 Cline 中設定自訂代理程式

1.  **自訂指令**：

    - 透過 Cline > 設定 > 自訂指令存取
    - 為您的代理程式提供行為指南

2.  **自訂工具整合**：

    - Cline 可以透過模型內容協定 (MCP) 擴展功能
    - 要求 Cline「新增工具」，它將建立一個針對您特定工作流程量身打造的新 MCP 伺服器
    - 自訂工具儲存在本地的 ~/Documents/Cline/MCP，方便與您的團隊共用

3.  **BMAD 方法實作**：

    - 為 BMAD 工作流程中的每個角色建立自訂工具
    - 組態針對每個角色特定的行為指南
    - 利用 Cline 的自主能力處理整個工作流程

4.  **模型選擇**：
    - 組態 Cline 以根據角色和任務複雜性使用不同的模型

### GitHub Copilot

#### 自訂代理程式組態 (即將推出)

GitHub Copilot 目前正在開發其 Copilot Extensions 系統，該系統將允許建立自訂代理程式/模式：

1.  **Copilot Extensions**：

    - 將 GitHub App 與 Copilot 代理程式結合以建立自訂功能
    - 允許開發人員直接在 Copilot Chat 中建置和整合自訂功能

2.  **建置自訂代理程式**：

    - 需要建立 GitHub App 並將其與 Copilot 代理程式整合
    - 自訂代理程式可以部署到可透過 HTTP 請求存取的伺服器

3.  **自訂指令**：
    - 目前支援用於指導一般行為的基本自訂指令
    - 完整的代理程式自訂支援正在開發中

_注意：GitHub Copilot 中的完整自訂模式組態仍在開發中。請查看 GitHub 的文件以獲取最新更新。_

## 任務

Tasks 可以複製到您的專案 docs/tasks 資料夾中，連同 checklists 和 templates。Tasks 的目的是減少一次性 IDE 代理程式的數量 - 您只需將 task 放入任何代理程式的聊天中，它就會執行該一次性 task。V3 之後將推出完整的工作流程 + task，並在此基礎上進行擴展 - 但 tasks 和 workflows 是一個強大的概念，它將使我們能夠為我們的代理程式建置許多功能，而無需在 IDE 中增加其整體程式設計和上下文的負擔 - 對於不經常使用的 tasks 尤其有用 - 類似於很少使用的 IDE 規則檔案。
