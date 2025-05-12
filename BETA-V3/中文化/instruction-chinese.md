# 指南

- [Web Agent 設定](#setting-up-web-mode-agents-in-gemini-gem-or-chatgpt-custom-gpt)
- [IDE Agent 設定](#ide-agent-setup)
- [Tasks 設定與使用](#tasks)

## 在 Gemini Gem 或 ChatGPT Custom GPT 設定 Web-Mode Agents

如需設定 web-mode agents，請參考下表。表格列出了 agent 名稱、描述檔案來源，以及需附加的 checklist 或 template 檔案。

| Agent Name         | Description File Path                           | Attachment Files (Checklists/Templates)                                                                                                                                        |
| ------------------ | ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0-bmad             | `BETA-V3/web-agent-modes/0-bmad.md`             |                                                                                                                                                                                |
| 1-analyst          | `BETA-V3/web-agent-modes/1-analyst.md`          | `BETA-V3/templates/project-brief-tmpl.txt`                                                                                                                                     |
| 2-pm               | `BETA-V3/web-agent-modes/2-pm.md`               | `BETA-V3/checklists/pm-checklist.txt`, `BETA-V3/templates/prd-tmpl.txt`                                                                                                        |
| 3-architect        | `BETA-V3/web-agent-modes/3-architect.md`        | `BETA-V3/templates/architecture-tmpl.txt`, `BETA-V3/checklists/architect-checklist.txt`                                                                                        |
| 4-design-architect | `BETA-V3/web-agent-modes/4-design-architect.md` | `BETA-V3/templates/front-end-architecture-tmpl.txt`, `BETA-V3/templates/front-end-spec-tmpl.txt`, `BETA-V3/checklists/frontend-architecture-checklist.txt`                     |
| 5-posm             | `BETA-V3/web-agent-modes/5-posm.md`             | `BETA-V3/checklists/po-master-checklist.txt`, `BETA-V3/templates/story-tmpl.txt`, `BETA-V3/checklists/story-draft-checklist.txt`, `BETA-V3/checklists/story-dod-checklist.txt` |

## IDE Agent 設定

V3 版本的 IDE Agents 均已最佳化至 6k 字元以內，以確保與 Windsurf 相容，並更適合於 IDE 使用！請確保您的 docs 資料夾內含有 templates/ 與 checklists/ 子資料夾。

### Cursor

目前 Cursor 僅支援最多 5 個自訂 agents，因此建議將可於 web 版本使用的 agents 優先設於 web 端，IDE 端則保留給最需要的 agent，例如 dev agent、sm agent。建議僅設定這些，以便日後可加入更專業的自訂開發 agent。

V3 引入了 Tasks，Workflows 也即將推出，屆時將允許更通用的 agile pro agent 處理目前多個 agents 所需的前置作業。

#### 在 Cursor 設定自訂模式

1. **進入 Agent 設定**：

   - 前往 Cursor Settings > Features > Chat & Composer
   - 找到 "Rules for AI" 區段，設定所有 agents 的基本規則

2. **建立自訂 Agents**：

   - 可建立並設定具特定工具、模型與自訂提示詞的 Custom Agents
   - Cursor 提供 GUI 介面建立自訂 agents
   - 參見 [Cursor Custom Modes doc](https://docs.cursor.com/chat/custom-modes#custom-modes)

3. **設定 BMAD Method Agents**：

   - 為 workflow 中每個 agent 定義專屬角色（Analyst、PM、Architect、PO/SM 等）
   - 指定每個 agent 可使用的工具（包括 Cursor 原生與 MCP 工具）
   - 設定自訂提示詞，定義 agent 的運作方式
   - 根據角色控制每個 agent 使用的模型
   - 設定其可與不可 YOLO 的範圍

### Windsurf

所有 V3 Agents 均已最佳化至 6K 字元限制，非常適合 Windsurf 使用！

#### 在 Windsurf 設定自訂模式

1. **進入 Agent 設定**：

   - 點擊右下角的 "Windsurf - Settings" 按鈕
   - 於設定面板內或右上角個人選單進入進階設定

2. **設定自訂規則**：

   - 為 Cascade（Windsurf 的 agentic chatbot）定義自訂 AI 規則
   - 指定 agents 回應方式、使用特定框架或遵循特定 API

3. **使用 Flows**：

   - Flows 結合 Agents 與 Copilots，打造完整 workflow
   - Windsurf Editor 專為能獨立處理複雜任務的 AI agents 設計
   - 使用 Model Context Protocol (MCP) 擴展 agent 能力

4. **BMAD Method 實作**：
   - 為 BMAD workflow 各角色建立自訂 agents
   - 為每個 agent 設定適當權限與能力
   - 善用 Windsurf 的 agentic 功能，維持 workflow 連貫性

### RooCode

#### 在 RooCode 設定自訂 Agents

1. **自訂模式設定**：

   - 透過設定檔建立專屬 AI 行為
   - 每個自訂模式可有專屬提示詞、檔案限制與自動核准設定

2. **建立 BMAD Method Agents**：

   - 為每個 BMAD 角色（Analyst、PM、Architect、PO/SM、Dev、Documentation 等）建立獨立模式
   - 針對角色自訂提示詞
   - 為每個角色設定適當的檔案限制（如 Architect 與 PM 模式僅可編輯 markdown 檔）
   - 設定直接切換模式，讓 agents 可根據需求切換

3. **模型設定**：

   - 為不同模式設定不同模型（如架構用進階模型，日常開發用經濟型模型）
   - RooCode 支援多家 API 提供者，包括 OpenRouter、Anthropic、OpenAI、Google Gemini、AWS Bedrock、Azure 及本地模型

4. **使用量追蹤**：
   - 監控每次 session 的 token 與成本用量
   - 根據任務複雜度最佳化模型選擇

### Cline

#### 在 Cline 設定自訂 Agents

1. **自訂指令**：

   - 進入 Cline > Settings > Custom Instructions
   - 為 agents 提供行為指引

2. **自訂工具整合**：

   - Cline 可透過 Model Context Protocol (MCP) 擴展能力
   - 請求 Cline「新增工具」即可建立專屬 workflow 的 MCP server
   - 自訂工具會儲存於 ~/Documents/Cline/MCP，方便團隊分享

3. **BMAD Method 實作**：

   - 為 BMAD workflow 各角色建立自訂工具
   - 為每個角色設定專屬行為指引
   - 善用 Cline 的自動化能力處理整個 workflow

4. **模型選擇**：
   - 根據角色與任務複雜度設定 Cline 使用不同模型

### GitHub Copilot

#### 自訂 Agent 設定（即將推出）

GitHub Copilot 正在開發 Copilot Extensions 系統，將支援自訂 agent/mode 建立：

1. **Copilot Extensions**：

   - 結合 GitHub App 與 Copilot agent，打造自訂功能
   - 允許開發者將自訂功能直接整合進 Copilot Chat

2. **建立自訂 Agents**：

   - 需建立 GitHub App 並與 Copilot agent 整合
   - 自訂 agents 可部署於可被 HTTP 請求存取的伺服器

3. **自訂指令**：
   - 目前支援基本自訂指令以引導一般行為
   - 完整 agent 自訂功能仍在開發中

_註：GitHub Copilot 的完整自訂模式設定仍在開發中，請參考 GitHub 官方文件以獲取最新資訊。_

## Tasks

Tasks 可直接複製到您的專案 docs/tasks 資料夾，並搭配 checklists 與 templates 使用。Tasks 旨在減少一次性 IDE agents 的需求——只需將 task 貼到任何 agent 的 chat，即可執行一次性任務。V3 之後將推出完整 workflow + task，進一步擴展此概念。Tasks 與 workflows 是強大的設計，能讓我們為 agents 建立豐富能力，而不必讓 IDE 內的 agent 程式與 context 膨脹——特別適合不常用的任務，類似於偶爾才用到的 ide 規則檔案。
