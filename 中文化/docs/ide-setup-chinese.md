# Agent 設定的 IDE 指南

主要建議在 Gemini Web 中使用 Uber Orchestrating BMad Agent，特別適用於處理簡報、PRD、高階 Epic、Story、網頁設計和提示輸出。但是，如果需要，所有操作也可以在 IDE 中完成，請參閱下方的 BMad Agent 設定部分。

## 單一 Agent

依照文件建立自訂模式，並從 personas 資料夾中貼上任何以 .ide.md 結尾的 agent。

## Tasks

由於 cursor 目前限制了允許的自訂模式總數 - 您可以利用 task 來處理您可能希望 agent 執行的一次性操作。只需將 task 拖曳到任何 agent 聊天視窗中，並要求 agent 完成該 task。

## BMad Agent

BMad Agent 需要將完整的 bmad agent 資料夾放置在專案的根目錄。設定 orchestrator 只需複製 ide-bmad-orchestrator.md 的 markdown 內容，方法與設定單一 Agent 相同。

## 在 Cursor 中設定自訂模式

若要使用自訂 agent 模式 - 請在此處查閱文件：https://docs.cursor.com/chat/custom-modes。

- 特別是，您需要在以下位置啟用自訂模式：設定 → 功能 → 聊天 → 自訂模式
- 可以使用特定的工具、模型和自訂提示來建立和設定自訂 Agent
- Cursor 允許透過 GUI 介面建立自訂 agent

Cursor 的注意事項：「我們正在考慮在您的專案中加入一個 .cursor/modes.json 檔案，以便更輕鬆地建立和分享自訂模式。」

## Windsurf

### 在 Windsurf 中設定自訂模式

1. **存取 Agent 設定**：

   - 按一下右下角的「Windsurf - 設定」按鈕
   - 透過設定面板中的按鈕或右上角的個人資料下拉式選單存取進階設定

2. **設定自訂規則**：

   - 為 Cascade (Windsurf 的 agentic 聊天機器人) 定義自訂 AI 規則
   - 指定 agent 應以某些方式回應、使用特定框架或遵循特定 API

3. **使用 Flows**：

   - Flows 結合 Agent 和 Copilot 以實現全面的工作流程
   - Windsurf 編輯器專為能夠獨立處理複雜任務的 AI agent 而設計
   - 使用 Model Context Protocol (MCP) 來擴展 agent 的能力

4. **BMAD 方法實作**：
   - 為 BMAD 工作流程中的每個角色建立自訂 agent
   - 為每個 agent 設定適當的權限和能力
   - 利用 Windsurf 的 agentic 功能來維持工作流程的連續性

## RooCode

### 在 RooCode 中設定自訂 Agent

1. **自訂模式設定**：

   - 透過設定檔建立量身打造的 AI 行為
   - 每個自訂模式都可以有特定的提示、檔案限制和自動核准設定

2. **建立 BMAD 方法 Agent**：

   - 為每個 BMAD 角色（分析師、PM、架構師、PO/SM、開發人員、文件等）建立不同的模式
   - 為每個模式自訂針對其角色的提示
   - 設定適合每個角色的檔案限制（例如，架構師和 PM 模式可以編輯 markdown 檔案）
   - 設定直接模式切換，以便 agent 在需要時可以請求切換到其他模式

3. **模型設定**：

   - 針對不同模式設定不同模型（例如，用於架構的進階模型 vs. 用於日常編碼任務的較便宜模型）
   - RooCode 支援多個 API 供應商，包括 OpenRouter、Anthropic、OpenAI、Google Gemini、AWS Bedrock、Azure 和本機模型

4. **用量追蹤**：
   - 監控每個工作階段的 token 和成本用量
   - 根據任務的複雜性優化模型選擇

## Cline

### 在 Cline 中設定自訂 Agent

1. **自訂指令**：

   - 透過 Cline > 設定 > 自訂指令存取
   - 為您的 agent 提供行為準則

2. **自訂工具整合**：

   - Cline 可以透過 Model Context Protocol (MCP) 擴展能力
   - 要求 Cline「新增工具」，它將建立一個針對您特定工作流程量身打造的新 MCP 伺服器
   - 自訂工具會儲存在本機的 ~/Documents/Cline/MCP，方便與您的團隊分享

3. **BMAD 方法實作**：

   - 為 BMAD 工作流程中的每個角色建立自訂工具
   - 設定針對每個角色的特定行為準則
   - 利用 Cline 的自主能力來處理整個工作流程

4. **模型選擇**：
   - 設定 Cline 以根據角色和任務複雜性使用不同的模型

## GitHub Copilot

### 自訂 Agent 設定 (即將推出)

https://github.com/microsoft/vscode-copilot-release/issues/9452

GitHub Copilot 目前正在開發其 Copilot Extensions 系統，該系統將允許建立自訂 agent/模式：

1. **Copilot Extensions**：

   - 將 GitHub App 與 Copilot agent 結合以建立自訂功能
   - 允許開發人員直接在 Copilot Chat 中建置和整合自訂功能

2. **建置自訂 Agent**：

   - 需要建立 GitHub App 並將其與 Copilot agent 整合
   - 自訂 agent 可以部署到可透過 HTTP 要求存取的伺服器

3. **自訂指令**：
   - 目前支援基本的自訂指令以引導一般行為
   - 完整的 agent 自訂支援正在開發中

_注意：GitHub Copilot 中的完整自訂模式設定仍在開發中。請查閱 GitHub 的文件以取得最新更新。_
