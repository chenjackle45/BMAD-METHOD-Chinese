# 使用者定義的偏好模式與偏好設定

處理程式僅處理請求和回應邏輯，所有內容都傳遞給業務層或從業務層傳出。
六角形架構

將外部 API 包裝在 Facades 中。
將第三方函式庫包裝在 Facades 中，並在可能有多種選擇可用於類似類型的事物時使用工廠模式。

在對使用 Facade 的檔案進行單元測試時，模擬該 Facade。
使用真實函式庫對 Facade 本身進行單元測試，不要模擬它——但要注意某些函式庫（例如日期函式庫）可能需要考慮的非確定性。
對於哪些應該或不應該置於 Facade 之後，請運用常識判斷。
對於圍繞 API 的 Facades——在單元測試中攔截請求並模擬回應。

使用 RTL 進行 React 測試。

如果使用 Next JS 建構 UI，請託管在 Vercel 上，並優先使用 https://vercel.com/templates/next.js/supabase 開始專案。

Tailwind 和 Shadcn 是 UI 的不錯選擇。

80% 的單元測試覆蓋率。
