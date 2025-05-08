# BMad 全流程代理寶石使用示範

**歡迎來到 BMad 方法 V2 的完整端到端操作演示！** 本示範展示了使用階段性代理協作進行 AI 輔助軟體開發的強大威力。你將看到每個專業代理（BA、PM、Architect、PO/SM）如何在專案生命週期的每個階段貢獻力量——從最初構想到可實作的計畫。

每個章節都包含了**完整 Gemini 互動紀錄連結**，讓你親眼見證人類與 AI 之間卓越的協作過程。demo 資料夾中保存了所有在代理間流轉的產出成果，構建出一條連貫的開發管道。

V2 方法的卓越之處在於代理以**互動式階段**運作，在重要決策點暫停等待你的回饋，而不是一次性丟出冗長文件。這創造了一個真正協作的體驗，你主導方向，AI 負責繁重細節。

請跟著流程，從概念到可實作的專案計畫，見證這個工作流如何徹底改變軟體開發！

## BA 腦力激盪

以下連結展示了與 BA 代理完整的對話過程，展現了這個驚人代理的多項功能。我一開始甚至不知道要做什麼，它幫我發想出有趣的教學題材，協助細化、深入研究（在思考模式下，沒有切換模型）、給出優秀的替代細節與想法，並逐段引導我最終產出簡報。效果出奇地好。完整對話與產出請見：

https://gemini.google.com/share/fec063449737

## PM 腦力激盪（其實不是 PM 哈哈）

我把上一步最後產出的 md 簡報和 PM 的提示語放到 Google 文件，方便與 PM 分享（其實也可以直接貼到新對話，不過用 Google Doc 開新檔、右鍵選「從 Markdown 貼上」，再點標題就會自動命名並儲存，真的很方便）。然後我用 Gemini 2.5 Pro 思考模式，附上 Google 文件並告訴 2-PM Gem 參考提示語開始對話。這是對話紀錄。後來發現我不小心把 BA 的提示也貼進 PM 的提示裡，結果反而產出了一份更精煉的簡報 2.0，笑死。

https://g.co/gemini/share/3e09f04138f2

然後我把這個產出又給 BA 跑一次，產生了新的版本與提示，如 [這個檔案](final-brief-with-pm-prompt.txt)（[md 版](final-brief-with-pm-prompt.md)）。

## PM 腦力激盪第二回

接下來流程我不再用 Google Docs（雖然比較方便），而是直接附上前一階段 txt 檔，否則連結無法分享。

這裡要注意的是，我不是被動接受的，你也不該只是旁觀——我在回答完初步問題後檢查它提出的 PRD 草案裡的 epics，很快就發現一個明顯的錯誤：它把檔案輸出與紀錄的 epic 放到最後，其實應該每個 epic 都要逐步做這件事。雖然 Architect 或 PO 應該會發現這問題，PM 也可能會在 checklist 階段發現，但如果你主動參與，成果會更快更好。

還有一點，這次我們給 PM 的簡報和提示語都很完整——它只問了一個問題就產出了第一版草案，超神！

PM 表現很棒，問對了問題，也產出了 [PRD 草案](prd.txt)（[md 版](prd.md)），以及每個 epic，[1](epic1.txt)（[md 版](epic1.md)），[2](epic2.txt)（[md 版](epic2.md)），[3](epic3.txt)（[md 版](epic3.md)），[4](epic4.txt)（[md 版](epic4.md)），[5](epic5.txt)（[md 版](epic5.md)）。

這些新 V2 代理最棒的地方就是它們會暫停，等你回答問題或逐段審查文件——比起一次收到大文件好多了。每個階段你都可以提問或要求更改——超容易、超有力！

草案完成後，它會執行 checklist——這是 V2 BMAD 方法另一個重大創新。等待 checklist 最終決策的時刻還挺令人期待的哈哈！

看到最終 PRD 與 EPIC 驗證總結全數通過，那種成就感超棒。

[完整對話紀錄請見](https://g.co/gemini/share/abbdff18316b)。

## Architect（糟糕的 Architect——已在第二回被炒掉）

我把 PRD 草案和 epics 交給 architect。我還是稱它們為草案，因為 architect 或 PO 可能還會發現問題或需要更新——但這麼簡單的專案應該不會有太大狀況。

我一開始就跟 architect 說：「回應提示在 PRD 最後的 'Initial Architect Prompt' 區段，我們現在進入架構設計模式——PM 規劃的所有 PRD 和 epics 都已附上」。

注意——這個 architect 直接一口氣產出所有東西並執行 checklist——未來要改進這個寶石和代理，讓它更符合流程！這是它一開始產出的[慘烈成果](botched-architecture.md)——別擔心，後來我修好了，第二回好很多！

有一點很麻煩，不管是 Gemini 還是 ChatGPT——如果 markdown 裡面又有 markdown 或 mermaid 區塊，輸出格式就會亂掉，因為 LLM 輸出的其實本來就是 markdown，只是 UI 在渲染！解決辦法很簡單——我跟它說「既然你預設就是 markdown 回應，就不要再用 markdown 區塊，直接用標準聊天輸出給我文件」——這招超有效，巢狀 markdown 都能正常包住！

我在這時候也更新了 agent，修正所有寶石的輸出格式，並調整 architect 讓它一份文件一份文件地逐步產出，每份都會提示我確認、說明取捨、或說明設計，等我確認每一份草稿後再問我要不要跑 checklist。改進後的用法請見下一節 Architect Take 2。

想看我跟這個爛 architect gem 的爆笑對話，請點[這裡](https://g.co/gemini/share/0a029a45d70b)。

（我已修正互動模式並加上 YOLO 模式，接著用改良版寶石重新開始第二回。）

## Architect Take 2（全新超棒的 architect）

這次用改良版 architect，初始提示一樣，再次送出第一個提示語，期待它不要再失控。

結果很成功——它確認不會再亂來！

我們的新 architect 超級棒，而且很有趣（「海盜口吻」Aye, yargs be a fine choice, matey!），炒掉舊 architect 真是明智！

它產出了[技術棧](tech-stack.txt)（[md 版](tech-stack.md)）——技術棧內容很棒，不像舊 architect 那樣模稜兩可！

我有提醒要明確說明不使用 axios 和 dotenv，避免 LLM 之後又用到。也建議加上 Winston，它還給了更簡單的 MVP 檔案日誌方案！現在真的是超級好幫手！真心希望再也不要見到舊 V1 architect，連掃地都不合格。

收到[專案結構文件](project-structure.txt)（[md 版](project-structure.md)）時我驚呆了——你可以在對話紀錄看到格式多棒——我直接複製整個回應貼到 md 檔，子區段完全沒問題，只要刪掉「這是你的檔案」那段就好！確認是 md 後，為了後續交接給 PO 也轉成 txt。

接下來 checklist 前，還有以下文件都是一份一份和我討論產出的：

- [Architecture](architecture.txt)（[md 版](architecture.md)）——「核心工作流／序列圖（主流程）」圖真的很厲害，有一個圖曾出現 mermaid bug——我已更新 agent 修正此類 bug，未來應該不會再出現——這是我遇過 LLM 最常見的 mermaid bug
- [Data Models](data-models.txt)（[md 版](data-models.md)）——又是一份複雜文件，直接複製回應貼進 markdown，完美！
- [API Reference](api-reference.txt)（[md 版](api-reference.md)）
- [Env Vars](environment-vars.txt)（[md 版](environment-vars.md)）
- [Coding Standards](coding-standards.txt)（[md 版](coding-standards.md)）
- [Testing Strategy](testing-strategy.txt)（[md 版](testing-strategy.md)）——這份真的超棒，還讓我確認有針對每個 epic 逐步測試的腳本，這點我也確認很讚！

最後它確認所有產出都完成，準備進行 checklist 評估！結果如下：5 個小問題與很棒的提問——有些我直接回答，有一題我不懂，它還詳細解釋給我聽！

我完成更新後，它還特別為這個專案建議新增一份文件！[Prompts](prompts.txt)（[md 版](prompts.md)）——我讓它幫忙確保這份文件在需要的地方（如 epic 4 與 coding standards）都有參照，也協助我重生 epic 4。

一切完成後，我跟它說做得很棒，它接下來給我的總結完美展現多階段代理的完整功能！片段如下：

```text
下一步：

專案現在可以進入實作階段，這些架構文件就是藍圖。開發代理可依定義的結構、標準與測試策略，實作每個 Epic 內的故事。

作為 3-Architect，如你需要進一步協助，我可切換到 Mode 3：Master Architect Advisory，提供下列支援：

說明架構概念。
協助解決實作挑戰。
評估潛在變更或增強。
協助管理技術債。
```

完整超大架構討論請見[這裡](https://g.co/gemini/share/4fedc292e068)！

## PO-SM

在 Gemini 2.5 Pro 的超大 100 萬 context 視窗下，這個模式真是太狂了！它能同時分析所有產出——因為附件有 10 個文件限制，我建立了 [Combined Artifacts](combined-artifacts-for-posm.txt)（[md 版](combined-artifacts-for-posm.md)），把全部 5 個 epic 文件都合併進去。

Checklist 執行時產出一大堆內容——驚人的是它真的全部考慮進去並產出這些結果（格式更漂亮的 markdown 我沒貼在這）。重點整理如下——它抓到兩個超棒的問題！一是 story 1.2 還要求 dotenv，但和 architect 決定後我們其實不用；二是它發現有提到 prompts.md 但我沒附上，也被它抓出來！所以我修正了 story 1.2，現在可以進入開發了！

驗證總結
分類狀態
分類 狀態 重大問題

1. 專案初始化與設定 PASS
2. 基礎設施與部署順序 PASS
3. 外部依賴與整合 PASS
4. 使用者／代理責任分工 PASS
5. 功能排序與依賴 PASS
6. MVP 範圍一致性 PASS
7. 風險管理與可行性 PASS
8. 文件與交接 PASS
9. MVP 後續考量 PASS
   全部通過 未發現重大缺陷。

匯出到 Sheets
重大缺陷
無
建議

雖然不是關鍵，Story 1.2 可以明確寫出 npm install {dotenv library} 而不是只提到套件名稱，雖然計畫已明確指定使用原生 Node.js .env 支援，不需要額外套件 [111, 549]。修正：計畫正確指出使用原生支援，無需外部套件 [549]，這個小問題可忽略。Story 1.2 提及安裝 dotenv [111-112]，與 environment-vars.txt [549]、tech-stack.txt [867] 內「使用原生支援、不需 dotenv」矛盾。應修正 Story 1.2。更新：檢查 Story 1.2 [111]，確實明確要求安裝 dotenv，這與 ENV [549]、ARCH [72]、TS [867]、PRD [752]（暗示 .env.example 原生支援）矛盾。需修正此不一致。決議：以 ENV、ARCH、TS、PRD 為正，Story 1.2 關於 dotenv 套件的需求是錯誤，後續將假設不需 dotenv 套件。

建議明確新增 docs/prompts.md 檔案，如 ARCH [89] 與 E4 Story 4.2 [321] 所述，即使一開始內容很簡單。更新：E4 Story 4.2 [321-325] 確實要求建立並參照 docs/prompts.md，這樣就夠了。

最終決策

核准：計畫內容完整、PRD、Architecture、Epics 與支援文件細緻詳盡。順序合理、依賴明確、風險有考慮、範圍緊扣 MVP 目標。技術約束被尊重，分階段測試大幅提升可測性與驗證性。dotenv 套件不一致問題已按多份文件參照假設為原生 Node.js 支援，計畫可進入實作。

可在用戶確認或自動進入時，切換到 SM 模式。

（完整對話連結見下一節）

## PO-SM 故事產生

這段純粹示範用，實際上我不喜歡這種做法，這時我已經想直接進專案了——但為了好玩與測試，我想驗證這個寶石能否產生高品質故事，甚至一次產生多個，適合像 taskmaster 那樣用。

產出看起來不錯，但我還是更喜歡在 IDE 用 Sonnet 3.5/3.7 與 SM 一次產生一個故事，然後再交給 Dev。主要是每個故事你可能都還想調整——但這只是個人偏好，這種一次產生全部故事的方法也許很適合你——歡迎試試看，有心得請告訴我！

- [Epic 1 故事草稿](epic-1-stories-demo.md)
- [Epic 2 故事草稿](epic-2-stories-demo.md)
- [Epic 3 故事草稿](epic-3-stories-demo.md)
  等等...

完整 [4-POSM 對話紀錄請見](https://g.co/gemini/share/9ab02d1baa18)。

如果你想看最終 app 建置的影片與成果，我會把連結貼在這裡——但我已經對這個 V2 規劃工作流的表現感到超級興奮！

感謝你看到這裡。

- BMad
