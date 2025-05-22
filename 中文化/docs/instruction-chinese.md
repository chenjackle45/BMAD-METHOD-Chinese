# 指示說明

- [Web Agent 設定](#setting-up-web-mode-agents-in-gemini-gem-or-chatgpt-custom-gpt)
- [IDE Agent 設定](#ide-agent-setup)
- [Tasks 設定與使用](#tasks)

## 設定 Web Agent Orchestrator

V3 中的 Agent Orchestrator 利用一個建置腳本將各種 agent 資產（personas、tasks、templates 等）打包成結構化格式，主要用於可利用大型內容視窗的網頁型 orchestrator agent。此過程包含將指定來源目錄中的檔案合併到打包的文字檔案中，並準備一個主要的 agent 提示。

### 總覽

建置過程由 `build-bmad-orchestrator.js` Node.js 腳本管理。此腳本從 `build-agent-cfg.js` 讀取其設定，處理資產目錄中的檔案，並將打包的資產輸出到指定的建置目錄。

快速入門：請參閱[下方說明](#running-the-build-script)

### 先決條件

- **Node.js**：確保您已安裝 Node.js 以執行建置腳本。Python 版本即將推出...

### 設定 (`build-agent-cfg.js`)

建置過程透過 `build-agent-cfg.js` 進行設定。主要參數包含：

- `orchestrator_agent_prompt`：指定 orchestrator agent 主要提示檔案的路徑，例如 `bmad-agent/web-bmad-orchestrator-agent.md`。此檔案將被複製到建置目錄中的 `agent-prompt.txt`。
  - 範例：`./bmad-agent/web-bmad-orchestrator-agent.md`
- `asset_root`：定義儲存 agent 資產的根目錄。腳本將在此路徑中尋找子目錄。
  - 範例：`./bmad-agent/` 意指它會在 `bmad-agent/` 中尋找 `personas`、`tasks` 等資料夾。
- `build_dir`：指定將建立打包輸出檔案和 `agent-prompt.txt` 的目錄。
  - 範例：`./bmad-agent/build/`
- `agent_cfg`：指定定義 Orchestrator 可體現之 agent 的 md cfg 檔案路徑。
  - 範例：`./bmad-agent/web-bmad-orchestrator-agent-cfg.md`

設定檔 (`build-agent-cfg.js`) 中的路徑是相對於 `BETA-V3` 目錄（`build-agent-cfg.js` 和建置腳本 `build-bmad-orchestrator.js` 所在的目錄）。

### 資產目錄結構

腳本預期在 `asset_root` 目錄中遵循特定的結構：

1.  **子目錄**：在 `asset_root` 下直接為每類資產建立子目錄。根據 `bmad-agent/` 資料夾，這些子目錄會是：
    - `checklists/`
    - `data/`
    - `personas/`
    - `tasks/`
    - `templates/`
2.  **資產檔案**：將您的個別資產檔案（例如 `.md`、`.txt`）放置在這些子目錄中。
    - 例如，persona 定義檔案應放入 `asset_root/personas/`，task 檔案應放入 `asset_root/tasks/` 等。
3.  **檔名唯一性**：在每個子目錄中，確保所有檔案都具有唯一的基礎名稱（即不含最終副檔名的檔名）。例如，在*同一個*子目錄（例如 `personas/`）中同時擁有 `my-persona.md` 和 `my-persona.txt` 會導致腳本因錯誤而停止。然而，`my-persona.md` 和 `another-persona.md` 則沒有問題。

### 執行建置腳本

注意：建置過程將跳過任何帶有 `.ide.<extension>` 的檔案 - 因此您也可以擁有 IDE 特定的 agent 或檔案，這些檔案對於網頁環境可能沒有意義，例如 `dev.ide.md` - 或特定 IDE 的 `sm.ide.md`。

1.  ```cmd
    node build-bmad-web-orchestrator.js
    ```

腳本將記錄其進度，包括發現的來源目錄、發現的任何問題（例如重複的基礎檔名）以及正在產生的輸出檔案。

### 輸出

執行腳本後，`build_dir`（例如 `bmad-agent/build/`）將包含：

1.  **打包的資產檔案**：對於在 `asset_root` 中處理的每個子目錄，將在 `build_dir` 中建立一個對應的 `.txt` 檔案。每個檔案都會串接其來源子目錄中所有檔案的內容。
    - 範例：來自 `asset_root/personas/` 的檔案將被打包到 `build_dir/personas.txt`。
    - 打包檔案中每個原始檔案的內容都以 `==================== START: [base_filename] ====================` 和 `==================== END: [base_filename] ====================` 分隔。
2.  **`agent-prompt.txt`**：此檔案是設定中由 `orchestrator_agent_prompt` 指定的 bmad orchestrator 提示的副本。
3.  **`agent-config.txt`**：這是關鍵檔案，讓 orchestrator 知道設定了哪些 agent 和 task，以及如何在編譯後的建置資產中找到 agent 的特定指示和 task。

這些打包的檔案和 agent 提示隨後即可供 Agent Orchestrator 使用。

### Gemini Gem 或 GPT 設定

`agent-prompt.txt` 中的文字會輸入到主要自訂網頁 agent 指示集視窗中。建置資料夾中的其他所有檔案都需要作為 Gem 或 GPT 的附件檔案。

### Orchestrator Agent 設定 (例如 `BETA-V3/bmad-agent/web-bmad-orchestrator-agent-cfg.md`)

雖然 `build-bmad-orchestrator.js` 會打包資產，但 Orchestrator 的核心行為、agent 定義和個性則定義在一個 Markdown 設定檔中。一個範例是 `bmad-agent/web-bmad-orchestrator-agent-cfg.md`（路徑相對於 `BETA-V3/`，在 `build-agent-cfg.js` 中透過 `agent_cfg` 指定）。此檔案是 Orchestrator 適應性的關鍵。

**主要功能與可設定性：**

- **Agent 定義**：Markdown 設定檔列出特化的 agent。每個 agent 的定義通常以其 `Title` 的層級 2 Markdown 標題開頭（例如 `## Title: Product Manager`）。然後列出屬性：

  - `Name`：（例如 `- Name: John`）- agent 的特定名稱。
  - `Description`：（例如 `- Description: "Details..."`）- agent 用途的簡要說明。
  - `Persona`：（例如 `- Persona: "personas#pm"`）- 定義核心個性和指示的參考（例如，參考 `personas.txt` 中的 `pm` 區段）。
  - `Customize`：（例如 `- Customize: "Behavior details..."`）- 用於特定的個性特徵或覆寫。如果發生衝突，此欄位的內容優先於基礎 `Persona`，詳情如 `bmad-agent/web-bmad-orchestrator-agent.md` 中所述。

  `checklists`、`templates`、`data`、`tasks`：這些鍵引入 agent 可存取資源的清單。每個項目都是對應鍵下的 Markdown 連結，例如：
  對於 `checklists`：

  ```markdown
  - checklists:
    - [Pm Checklist](checklists#pm-checklist)
    - [Another Checklist](checklists#another-one)
  ```

  對於 `tasks`：

  ```markdown
  - tasks:
    - [Create Prd](tasks#create-prd)
  ```

  這些參考（例如 `checklists#pm-checklist` 或 `tasks#create-prd`）指向打包資產檔案中的區段，為 agent 提供其知識和工具。注意：使用 `data`（而非 `data_sources`），並使用 `tasks`（而非舊版文件樣式中的 `available_tasks`）。

  - `Operating Modes`：（例如 `- Operating Modes:
  - "Mode1"
  - "Mode2"`）- 定義操作模式/階段。
  - `Interaction Modes`：（例如 `- Interaction Modes:
  - "Interactive"
  - "YOLO"`）- 指定互動風格。

**運作方式 (來自 `orchestrator-agent.md` 的概念流程)：**

1.  Orchestrator（最初為 BMad）載入並解析 Markdown agent 設定檔（例如 `web-bmad-orchestrator-agent-cfg.md`）。
2.  當使用者請求符合 agent 的 `title`、`name`、`description` 或 `classification_label` 時，Orchestrator 會識別目標 agent。
3.  然後它會透過以下方式載入 agent 的 `persona` 以及任何相關的 `templates`、`checklists`、`data_sources` 和 `tasks`：
    - 識別正確的打包 `.txt` 檔案（例如，`personas#pm` 對應 `personas.txt`）。
    - 提取特定的內容區塊（例如，從 `personas.txt` 中提取 `pm` 區段）。
4.  套用 Markdown 設定中的 `Customize` 指示，可能會修改 agent 的行為。
5.  Orchestrator 隨後*成為*該 agent，採用其在 Markdown 設定和載入的資產區段中定義的完整 persona、知識和操作參數。

此系統使 Agent Orchestrator 高度適應。您可以輕鬆定義新的 agent、修改現有的 agent、使用 `Customize` 欄位（在 Markdown agent 設定檔如 `web-bmad-orchestrator-agent-cfg.md` 中）調整個性，或變更其知識庫、主要提示和資產路徑（在 `build-agent-cfg.js` 和對應的資產檔案中），然後在資產內容變更時重新執行建置腳本。

## IDE Agent 設定與使用

V3 中的 IDE Agent 專為在 Windsurf 和 Cursor 等 IDE 環境中獲得最佳效能而設計，著重於較小的 agent 大小和高效的內容管理。

### 獨立 IDE Agent

您可以使用特化的獨立 IDE agent，例如 `sm.ide.md` (Scrum Master) 和 `dev.ide.md` (Developer)，用於特定角色，如 story 產生或開發 task。這些 agent 或任何一般的 IDE agent 也可以透過向 agent 提供您 `docs/tasks/` 資料夾中的 task 定義來直接參考和執行 task。

### IDE Agent Orchestrator (`ide-bmad-orchestrator.md`)

一個強大的替代方案是 `ide-bmad-orchestrator.md`。此 agent 提供了網頁 orchestrator 的靈活性——允許單一 IDE agent 體現多個 personas——但**無需任何建置步驟。**它會動態載入其設定和所有相關資源。

#### IDE Orchestrator 如何運作

1.  **設定 (`ide-bmad-orchestrator-cfg.md`):**
    orchestrator 的行為主要由一個 Markdown 設定檔驅動（例如 `BETA-V3/bmad-agent/ide-bmad-orchestrator-cfg.md`，其路徑在 `ide-bmad-orchestrator.md` 本身中指定）。此設定檔有兩個主要部分：

    - **Data Resolution:**
      位於設定檔頂部，此區段定義基礎路徑的鍵值對。這些路徑告訴 orchestrator 在何處尋找不同類型的資產檔案（personas、tasks、checklists、templates、data）。

      ```markdown
      # IDE Agent 設定

      ## Data Resolution

      agent-root: (project-root)/BETA-V3/bmad-agent
      checklists: (agent-root)/checklists
      data: (agent-root)/data
      personas: (agent-root)/personas
      tasks: (agent-root)/tasks
      templates: (agent-root)/templates

      注意：除非提供特定路徑，否則所有 Persona 參考和 task markdown 樣式連結都假定這些 data resolution 路徑。
      範例：如果上述 cfg 具有 `agent-root: root/foo/` 和 `tasks: (agent-root)/tasks`，則下方的 [Create PRD](create-prd.md) 將解析為 `root/foo/tasks/create-prd.md`
      ```

      `(project-root)` 預留位置通常被解譯為您目前工作區的根目錄。

    - **Agent 定義:**
      在 `Data Resolution` 區段之後，檔案列出 orchestrator 可以成為的每個特化 agent 的定義。每個 agent 通常以 `## Title:` Markdown 標題引入。
      每個 agent 的主要屬性包括：

      - `Name`：agent 的特定名稱（例如 `- Name: Larry`）。
      - `Customize`：提供 agent 特定個性特徵或行為覆寫的字串（例如 `- Customize: "You are a bit of a know-it-all..."`）。
      - `Description`：agent 角色和功能的簡要摘要。
      - `Persona`：包含 agent 核心 persona 定義的 Markdown 檔案名稱（例如 `- Persona: "analyst.md"`）。此檔案使用 `Data Resolution` 區段中的 `personas:` 路徑定位。
      - `Tasks`：agent 可以執行的 task 清單。每個 task 都是一個 Markdown 連結：

        - 連結文字是使用者易懂的 task 名稱（例如 `[Create PRD]`）。
        - 連結目標是外部 task 定義的 Markdown 檔名（例如 `(create-prd.md)`），使用 `tasks:` 路徑解析，或是一個特殊字串，如 `(In Analyst Memory Already)`，表示 task 邏輯是 persona 主要定義的一部分。
          範例：

        ```markdown
        ## Title: Product Owner AKA PO

        - Name: Curly
        - Persona: "po.md"
        - Tasks:
          - [Create PRD](create-prd.md)
          - [Create Next Story](create-next-story-task.md)
        ```

2.  **操作工作流程 (在 `ide-bmad-orchestrator.md` 內部):**
    - **初始化：** 在您的 IDE 中啟動後，`ide-bmad-orchestrator.md` 首先會載入並解析其指定的設定檔 (`ide-bmad-orchestrator-cfg.md`)。如果失敗，它會通知您並停止。
    - **問候與 Persona 清單：** 它會向您問候。如果您的初始指令不清楚或您詢問，它將列出可用的專家 personas（依 `Title`、`Name` 和 `Description`）以及每個 persona 可以執行的 `Tasks`，所有這些都來自載入的設定。
    - **Persona 啟動：** 當您請求特定的 persona（例如「成為 Analyst」或「我需要 Larry 協助研究」）時，orchestrator 會：
      - 在其設定中找到該 persona。
      - 載入對應的 persona 檔案（例如 `analyst.md`）。
      - 套用任何 `Customize:` 指示。
      - 宣布啟動（例如「正在啟動 Analyst (Larry)...」）。
      - **然後 orchestrator 會完全體現所選的 agent。**其原始的 orchestrator persona 將進入休眠狀態。
    - **Task 執行：** 一旦 persona 啟動，它將嘗試將您的請求與其設定的 `Tasks` 之一進行比對。
      - 如果 task 參考外部檔案（例如 `create-prd.md`），則會載入該檔案並遵循其指示。啟動的 persona 將使用主要設定中的 `Data Resolution` 路徑來尋找 task 檔案中提及的任何相依檔案，例如 templates 或 checklists。
      - 如果 task 標記為「In Memory」（或類似標記），則啟動的 persona 會根據其內部定義執行它。
    - **內容與 Persona 切換：** orchestrator 一次只體現一個 persona。如果您在某個 persona 啟動時要求切換到不同的 persona，它通常會建議開始一個新的聊天工作階段以保持清晰的內容。但是，如果您堅持在同一個聊天中切換 personas，它允許一個明確的「覆寫安全協定」指令。這會終止目前的 persona 並使用新的 persona 重新初始化。

#### IDE Orchestrator 使用說明

1.  **設定您的組態 (`ide-bmad-orchestrator-cfg.md`):**
    - 確保您有一個 `ide-bmad-orchestrator-cfg.md` 檔案。您可以使用位於 `BETA-V3/bmad-agent/` 中的檔案作為範本或起點。
    - 驗證頂部的 `Data Resolution` 路徑是否正確指向您相對於專案結構的資產資料夾（personas、tasks、templates、checklists、data）。
    - 使用其 `Title`、`Name`、`Customize` 指示、`Persona` 檔案和 `Tasks` 定義您想要的 agent。確保參考的 persona 和 task 檔案存在於您的 `Data Resolution` 路徑指定的位置。
2.  **設定您的 persona 和 task 檔案：**
    - 在您的 `personas` 目錄中為每個 persona 建立 Markdown 檔案（例如 `analyst.md`、`po.md`）。
    - 在您的 `tasks` 目錄中為每個 task 建立 Markdown 檔案（例如 `create-prd.md`）。
3.  **啟動 Orchestrator：**
    - 在您的 IDE（例如 Cursor）中，選擇 `ide-bmad-orchestrator.md` 檔案/agent 作為您啟動的 AI 助理。
4.  **與 Orchestrator 互動：**
    - **初始互動：**
      - orchestrator 會向您問候並確認已載入其設定。
      - 您可以問：「有哪些 agent 可用？」或「列出 personas 和 tasks。」
    - **啟動 Persona：**
      - 告訴 orchestrator 您想要哪個 persona：「我想與 Product Owner 合作」，或「啟動 Curly」，或「成為 PO」。
    - **執行 Task：**
      - 一旦 persona 啟動，請說明 task：「建立一個 PRD」，或者如果 persona 是「Curly」(PO)，您可以說「Curly，建立下一個 story」。
      - 您也可以結合 persona 啟動和 task 請求：「Curly，我需要您建立一個 PRD。」
    - **切換 Personas：**
      - 如果您需要切換：「我現在需要與 Architect 對話。」
      - orchestrator 會建議一個新的聊天。如果您想在目前的聊天中切換，您需要在提示時給出明確的覆寫指令（例如「覆寫安全協定並切換到 Architect」）。
    - **遵循 Persona 指示：** 一旦 persona 啟動，它將根據其定義和正在執行的 task 引導您。請記住，task 參考的資源檔案（如 templates 或 checklists）將使用 `ide-bmad-orchestrator-cfg.md` 中的全域 `Data Resolution` 路徑進行解析。

此設定允許在您的 IDE 中直接使用高度彈性且動態設定的多 persona agent，從而簡化各種開發和專案管理工作流程。

## Tasks

Tasks 可以連同 checklists 和 templates 一起複製到您專案的 docs/tasks 資料夾中。Tasks 的目的是減少一次性 IDE agent 的數量 - 您只需將 task 拖放到任何 agent 的聊天中，它就會執行該一次性 task。V3 之後將推出完整的 workflow + task 功能，並對此進行擴展 - 但 tasks 和 workflows 是一個強大的概念，它將使我們能夠為我們的 agent 建置許多功能，而無需在 IDE 中膨脹其整體程式設計和內容 - 這對於不常使用的 tasks 特別有用 - 類似於很少使用的 IDE 規則檔案。
