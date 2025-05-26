# Frontend Architecture Document Review Checklist

## 目的

本檢查表供 Design Architect 於完成「Frontend Architecture Mode」並填寫 `front-end-architecture-tmpl.txt`（或 `.md`）文件後使用。其目的是確保所有章節皆有完整覆蓋，並於最終定稿前符合品質標準。

---

## I. 簡介

- [ ] 是否於簡介中正確填寫 `{Project Name}`？
- [ ] 是否有主架構文件的連結且正確？
- [ ] 是否有 UI/UX Specification 的連結且正確？
- [ ] 是否有主要設計檔案（Figma、Sketch 等）的連結且正確？
- [ ] 若適用且可用，是否包含已部署的 Storybook／Component Showcase 連結？

## II. 整體前端理念與模式

- [ ] 是否明確說明所選 Framework 及 Core Libraries，且與主架構文件一致？
- [ ] 是否清楚描述 Component Architecture（如 Atomic Design、Presentational/Container）？
- [ ] 是否有高層次說明 State Management Strategy（如 Redux Toolkit、Zustand）？
- [ ] 是否清楚解釋 Data Flow（如 Unidirectional）？
- [ ] 是否明確定義 Styling Approach（如 CSS Modules、Tailwind CSS）？
- [ ] 是否列出將採用的 Key Design Patterns（如 Provider、Hooks）？
- [ ] 本節內容是否與主架構文件中的「Definitive Tech Stack Selections」一致？
- [ ] 是否有考量整體系統架構（monorepo/polyrepo、backend services）之影響？

## III. 前端目錄結構詳述

- [ ] 是否提供前端應用程式資料夾結構的 ASCII 圖？
- [ ] 圖示是否清晰、準確，並反映所選框架／模式？
- [ ] 是否有強調組件、頁面、服務、state、樣式等的組織慣例？
- [ ] 是否有說明特定慣例或結構設計理由的註解，且內容清楚？

## IV. 組件拆解與實作細節

### 組件命名與組織

- [ ] 是否有描述組件命名慣例（如 PascalCase）？
- [ ] 是否明確說明組件於檔案系統中的組織方式（如有需要可重申目錄結構）？

### Component Specification 範本

- [ ] 「Template for Component Specification」本身是否完整且定義明確？
  - [ ] 是否包含：Purpose、Source File(s)、Visual Reference 等欄位？
  - [ ] 是否有 Props 表格結構（Name、Type、Required、Default、Description）？
  - [ ] 是否有 Internal State 表格結構（Variable、Type、Initial Value、Description）？
  - [ ] 是否有 Key UI Elements／Structure 區塊（文字描述或 pseudo-HTML）？
  - [ ] 是否有 Events Handled／Emitted 區塊？
  - [ ] 是否有 Actions Triggered（State Management、API Calls）區塊？
  - [ ] 是否有 Styling Notes 區塊？
  - [ ] 是否有 Accessibility Notes 區塊？
- [ ] 是否明確說明此範本應用於大多數功能性組件？

### 基礎／共用組件（如有預先指定）

- [ ] 若有指定基礎／共用 UI 組件，是否皆遵循「Template for Component Specification」？
- [ ] 是否清楚說明預先指定這些組件的理由？

## V. State Management 深入說明

- [ ] 是否重申所選 State Management Solution，並簡要說明理由（若主架構文件未完全涵蓋）？
- [ ] 是否明確定義 Store Structure／Slices 的慣例（如位置、依功能分 Slice）？
- [ ] 若有提供 Core Slice 範例（如 `sessionSlice`）：
  - [ ] 是否明確說明其用途？
  - [ ] 是否定義其 State Shape（如 TypeScript interface）？
  - [ ] 是否列出其 Key Reducers／Actions？
- [ ] 是否有 Feature Slice 範本，說明用途、state shape 及主要 reducers/actions？
- [ ] 是否有註明 Key Selectors 的慣例（如使用 `createSelector`）？
- [ ] 是否有提供核心 slice 的 Key Selectors 範例？
- [ ] 是否有描述 Key Actions／Reducers／Thunks（特別是 async）的慣例？
- [ ] 是否有提供 Core Action/Thunk（如 `authenticateUser`）範例，並說明其用途及 dispatch 流程？
- [ ] 是否有 Feature Action/Thunk 範本，供功能性 async 操作參考？

## VI. API 互動層

- [ ] 是否詳細說明 HTTP Client 設定（如 Axios instance、Fetch wrapper、base URL、default headers、interceptors）？
- [ ] 是否說明 Service Definitions 的慣例？
- [ ] 是否有提供 service 範例（如 `userService.ts`），並說明其用途及範例函式？
- [ ] 是否描述 API 呼叫的全域錯誤處理（如 toast 通知、全域錯誤 state）？
- [ ] 是否有針對元件內的特定錯誤處理提供指引？
- [ ] 是否有詳細說明並設定 API 呼叫的 client-side Retry Logic？

## VII. 路由策略

- [ ] 是否明確說明所選 Routing Library？
- [ ] 是否有提供 Route Definitions 表格？
  - [ ] 是否包含 Path Pattern、Component/Page、Protection status 及每條路由的說明？
  - [ ] 是否列出所有主要應用程式路由？
- [ ] 是否有描述用於保護路由的 Authentication Guard 機制？
- [ ] 若有角色／權限需求，是否有描述 Authorization Guard 機制？

## VIII. 建置、打包與部署

- [ ] 是否列出主要 Build Scripts（如 `npm run build`），並說明其用途？
- [ ] 是否說明於不同環境下建置流程中 Environment Variables 的處理方式？
- [ ] 是否詳細說明 Code Splitting 策略（如依路由、依組件）？
- [ ] 是否確認或說明 Tree Shaking？
- [ ] 是否有說明 Lazy Loading 策略（針對組件、圖片、路由）？
- [ ] 是否有提及 Build 工具的 Minification & Compression？
- [ ] 是否明確指定 Target Deployment Platform（如 Vercel、Netlify）？
- [ ] 是否說明 Deployment Trigger（如 Git push 觸發 CI/CD），並參照主 CI/CD pipeline？
- [ ] 是否有說明靜態資產的 Asset Caching Strategy（CDN／瀏覽器）？

## IX. 前端測試策略

- [ ] 是否有主測試策略文件／章節的連結，且正確？
- [ ] 關於 Component Testing：
  - [ ] 是否明確定義測試範圍？
  - [ ] 是否列出所用工具？
  - [ ] 是否清楚說明測試重點（rendering、props、interactions）？
  - [ ] 是否指定測試檔案位置？
- [ ] 關於 UI Integration／Flow Testing：
  - [ ] 是否明確說明測試範圍（多組件間互動）？
  - [ ] 是否列出所用工具（可與 component testing 相同）？
  - [ ] 是否清楚說明測試重點？
- [ ] 關於 End-to-End UI Testing：
  - [ ] 是否重申主策略中的工具（如 Playwright、Cypress）？
  - [ ] 是否定義測試範圍（前端主要使用者流程）？
  - [ ] 是否有說明 UI E2E 測試的 Test Data Management？

## X. 無障礙（AX）實作細節

- [ ] 是否強調使用 Semantic HTML？
- [ ] 是否有提供 ARIA Implementation 指引（自訂元件的 roles、states、properties）？
- [ ] 是否明確說明 Keyboard Navigation 要求（所有互動元素皆可聚焦／操作）？
- [ ] 是否有涵蓋 Focus Management（如 modal、動態內容）？
- [ ] 是否列出 AX 測試工具（如 Axe DevTools、Lighthouse）？
- [ ] 本節內容是否與 UI/UX Specification 的 AX 要求一致？

## XI. 效能考量

- [ ] 是否有討論 Image Optimization（格式、響應式圖片、lazy loading）？
- [ ] 若有需要，是否重申 Code Splitting & Lazy Loading（對感知效能的影響）？
- [ ] 是否有提及減少重繪的技巧（如 `React.memo`）？
- [ ] 是否有考慮事件處理的 Debouncing／Throttling？
- [ ] 若適用，是否有提及長列表／大量資料的 Virtualization？
- [ ] 若相關，是否有討論 Client-Side Caching Strategies（瀏覽器快取、service workers）？
- [ ] 是否列出效能監控工具（如 Lighthouse、DevTools）？

## XII. 變更紀錄

- [ ] 是否有變更紀錄表格並已初始化？
- [ ] 是否有文件演進時更新變更紀錄的流程？

---

## 最終審查簽核

- [ ] 是否已填寫或移除所有 placeholder（如 `{Project Name}`、`{e.g., ...}`）？
- [ ] Design Architect 是否已審查文件之清晰度、一致性與完整性？
- [ ] 所有連結文件（Main Architecture、UI/UX Spec）是否已定稿或穩定，可供本文件依據？
- [ ] 文件是否已準備好可提供開發團隊參考？
