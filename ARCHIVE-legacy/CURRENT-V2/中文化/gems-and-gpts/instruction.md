# 指令說明

## Gemini Gem 2.5

- https://gemini.google.com/gems/view
- Client + New Gem
- 名稱：建議從數字或獨特字母開頭，這樣能更容易識別 Gem。例如：1-Analyst、2-PM 等。
- 指令：將對應的 gem.md 檔案內容完整貼上
- 知識來源：為該角色指定的 Text 檔案（見下方說明）以及其他你想提供的補充資料。例如，如果你知道 architect 一定會或應該使用特定架構，可以提供一份建議架構或技術棧的文件，這樣每次就不需要重新說明。不過你也可以選擇保持簡單，只使用這個 repo 中的檔案。

### Analyst（BA/RA）

- 指令：貼上 `1-analyst-gem.md` 的內容
- 知識來源：`templates/project-brief.txt`
- 對話中使用：建議使用 Mode 1 搭配 2.5 Pro Deep Research。Mode 2 建議搭配 2.5 Pro Thinking Mode，可選擇性附上 Mode 1 的深度研究。

### Product Manager（PM）

- 指令：貼上 `2-pm-gem.md` 的內容
- 知識來源：`templates/prd.txt`、`templates/epicN.txt`、`templates/ui-ux-spec.txt`、`templates/pm-checklist.txt`
- 對話中使用：Mode 1 建議使用 2.5 Pro Deep Research，Mode 2 建議使用 2.5 Pro Thinking Mode。對話一開始建議也附上產品簡報（product brief）。

### Architect

- 指令：貼上 `3-architect-gem.md` 的內容
- 知識來源：`templates/architecture-templates.txt`、`templates/architect-checklist.txt`
- 對話中使用：Mode 1 建議使用 2.5 Pro Deep Research，Mode 2 建議使用 2.5 Pro Thinking Mode。對話一開始建議附上產品簡報、PRD 和任何產出的 Epic 檔案。如果已在 Mode 1 做過架構的深度研究，也要一併附上。如果 PRD 中有未充分展開的深度技術細節，也應提供給 architect。

### PO + SM

- 指令：貼上 `4-po-sm-gem.md` 的內容
- 知識來源：`templates/story-template.txt`、`templates/po-checklist.txt`
- 此 Gem 屬於選用，與 IDE 中的流程不同，這版本會一次產出所有剩餘的故事內容，而不是每完成一項就產出一項。它有一個主要用途是：
  - 可以將產出內容傳給此 PO + SM 的 gem 或自訂 GPT，請其深入分析所有細節以找出潛在問題、缺口或不一致處。我尚未完整測試這個用途，因為我偏好一次只生成並完成一則故事。但這是一個值得探索的方向。
- 對話中使用：建議一開始就提供前一階段所有可能的產出文件。若檔案數量達上限，可用 Thinking Mode 附上整個資料夾（2.5 Pro），或將多份文件合併。SM 需要的文件包含最新版本的 `prd.md`、`architecture.md`、技術擴充過的 `epicN.md...`、以及架構中提及的參考文件，這些通常是在 PM/Architect 初步協作後產出與修訂完成的。
- IDE 版本（位於 agents 資料夾）是針對開發者每次生成一個故事來使用。本 Gem 則會一次產出完整文件，供後續逐一實作使用。
