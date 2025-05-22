# {Project Name} 前端架構文件

## 目錄

{ 若有新增或移除章節，請更新此處 }

- [簡介](#introduction)
- [整體前端理念與模式](#overall-frontend-philosophy--patterns)
- [詳細前端目錄結構](#detailed-frontend-directory-structure)
- [組件拆解與實作細節](#component-breakdown--implementation-details)
  - [組件命名與組織](#component-naming--organization)
  - [組件規格範本](#template-for-component-specification)
- [深入狀態管理](#state-management-in-depth)
  - [Store 結構 / Slices](#store-structure--slices)
  - [關鍵 Selectors](#key-selectors)
  - [關鍵 Actions / Reducers / Thunks](#key-actions--reducers--thunks)
- [API 互動層](#api-interaction-layer)
  - [Client/Service 結構](#clientservice-structure)
  - [錯誤處理與重試 (前端)](#error-handling--retries-frontend)
- [路由策略](#routing-strategy)
  - [路由定義](#route-definitions)
  - [路由守衛 / 保護](#route-guards--protection)
- [建置、打包與部署](#build-bundling-and-deployment)
  - [建置流程與腳本](#build-process--scripts)
  - [關鍵打包優化](#key-bundling-optimizations)
  - [部署至 CDN/託管](#deployment-to-cdnhosting)
- [前端測試策略](#frontend-testing-strategy)
  - [組件測試](#component-testing)
  - [UI 整合/流程測試](#ui-integrationflow-testing)
  - [端對端 UI 測試工具與範圍](#end-to-end-ui-testing-tools--scope)
- [無障礙 (AX) 實作細節](#accessibility-ax-implementation-details)
- [效能考量](#performance-considerations)
- [國際化 (i18n) 與本地化 (l10n) 策略](#internationalization-i18n-and-localization-l10n-strategy)
- [功能旗標管理](#feature-flag-management)
- [前端安全性考量](#frontend-security-considerations)
- [瀏覽器支援與漸進式增強](#browser-support-and-progressive-enhancement)
- [變更日誌](#change-log)

## 簡介

{ 本文件詳細說明 {Project Name} 前端的技術架構。它是主要 {Project Name} 架構文件和 UI/UX 規格書的補充。本文件詳細說明前端架構，並**基於主要 {Project Name} 架構文件（`docs/architecture.md` 或連結的等效文件）中定義的基礎決策**（例如，整體技術堆疊、CI/CD、主要測試工具）。**任何前端特定的闡述或與通用模式的偏差都必須在此明確註明。**目標是為前端開發提供清晰的藍圖，確保一致性、可維護性，並與整體系統設計和使用者體驗目標保持一致。 }

- **主要架構文件連結 (必要):** {例如，`docs/architecture.md`}
- **UI/UX 規格書連結 (若存在則必要):** {例如，`docs/front-end-spec.md`}
- **主要設計檔案連結 (Figma, Sketch 等) (若存在則必要):** {來自 UI/UX 規格書}
- **已部署 Storybook / 組件展示連結 (若適用):** {URL}

## 整體前端理念與模式

{ 描述為前端選擇的核心架構決策和模式。這應與主要架構文件中的「最終技術堆疊選擇」保持一致，並考慮整體系統架構的影響（例如，monorepo 與 polyrepo、後端服務結構）。 }

- **框架與核心函式庫:** {例如，React 18.x 搭配 Next.js 13.x、Angular 16.x、Vue 3.x 搭配 Nuxt.js}。**這些源自主要架構文件中的「最終技術堆疊選擇」。**本節詳細說明這些選擇*如何*具體應用於前端。
- **組件架構:** {例如，Atomic Design 原則、Presentational 與 Container 組件、使用特定組件函式庫如 Material UI、Tailwind CSS 的樣式方法。指定選擇的方法和任何關鍵函式庫。}
- **狀態管理策略:** {例如，Redux Toolkit、Zustand、Vuex、NgRx。簡要描述整體方法——全域 store、功能 stores、context API 使用。**參考主要架構文件並在「深入狀態管理」一節中進一步詳細說明。**}
- **資料流:** {例如，單向資料流 (Flux/Redux 模式)、React Query/SWR 用於伺服器狀態。描述資料如何擷取、快取、傳遞給組件以及更新。}
- **樣式方法:** **{選擇的樣式解決方案，例如 Tailwind CSS / CSS Modules / Styled Components}**。設定檔：{例如，`tailwind.config.js`、`postcss.config.js`}。關鍵慣例：{例如，「Tailwind 的 Utility-first 方法。自訂組件定義在 `src/styles/components.css` 中。主題擴展在 `tailwind.config.js` 的 `theme.extend` 下。對於 CSS Modules，檔案與組件共置，例如 `MyComponent.module.css`。」}
- **使用的關鍵設計模式:** {例如，Provider 模式、Hooks、Higher-Order Components、用於 API 呼叫的 Service 模式、Container/Presentational。這些模式必須一致應用。任何偏差都需要理由和文件記錄。}

## 詳細前端目錄結構

{ 提供一個 ASCII 圖表，表示前端應用程式的特定資料夾結構（例如，在 `src/` 或 `app/` 內，或者如果是 monorepo 的一部分，則在專用的 `frontend/` 根目錄中）。這應該詳細說明架構文件中概述的主要專案結構的前端部分。強調組織組件、頁面/視圖、服務、狀態、樣式、資產等的慣例。對於每個關鍵目錄，提供一句關於其用途的強制性描述。}

### 範例 - 非規範性 (適用於 React/Next.js 應用程式)

```plaintext
src/
├── app/                        # Next.js App Router：頁面/佈局/路由。必須包含路由區段、佈局和頁面組件。
│   ├── (features)/             # 基於功能的路由群組。必須將特定功能的相關路由分組。
│   │   └── dashboard/
│   │       ├── layout.tsx      # 特定於儀表板功能路由的佈局。
│   │       └── page.tsx        # 儀表板路由的進入頁面組件。
│   ├── api/                    # API 路由 (若使用 Next.js 後端功能)。必須包含客戶端呼叫的後端處理常式。
│   ├── globals.css             # 全域樣式。必須包含基礎樣式、CSS 變數定義、Tailwind base/components/utilities。
│   └── layout.tsx              # 整個應用程式的根佈局。
├── components/                 # 共用/可重複使用的 UI 組件。
│   ├── ui/                     # 基礎 UI 元素 (Button, Input, Card)。必須僅包含通用的、可重複使用的、展示性的 UI 元素，通常對應自設計系統。不得包含業務邏輯。
│   │   ├── Button.tsx
│   │   └── ...
│   ├── layout/                 # 佈局組件 (Header, Footer, Sidebar)。必須包含結構化頁面佈局的組件，而非特定頁面內容。
│   │   ├── Header.tsx
│   │   └── ...
│   └── (feature-specific)/     # 特定於某功能但可能在該功能內重複使用的組件。這是與 features/ 目錄中共置的替代方案。
│       └── user-profile/
│           └── ProfileCard.tsx
├── hooks/                      # 全域/可共用的自訂 React Hooks。必須是通用的，且可供多個功能/組件使用。
│   └── useAuth.ts
├── lib/ / utils/             # 工具函式、輔助函式、常數。必須包含純函式和常數，除非明確命名（例如 `react-helpers.ts`），否則不得包含副作用或框架特定程式碼。
│   └── utils.ts
├── services/                   # 全域 API 服務客戶端或 SDK 設定。必須定義基礎 API 客戶端實例和核心資料擷取/變動服務。
│   └── apiClient.ts
├── store/                      # 全域狀態管理設定 (例如，Redux store、Zustand store)。
│   ├── index.ts                # 主要 store 設定和匯出。
│   ├── rootReducer.ts          # 若使用 Redux，則為根 reducer。
│   └── (slices)/               # 全域狀態 slices 的目錄 (若未與功能共置)。
├── styles/                     # 全域樣式、主題設定 (若未使用 `globals.css` 或類似檔案，或用於特定樣式系統如 SCSS partials)。
└── types/                      # 全域 TypeScript 型別定義/介面。必須包含跨多個功能/模組共用的型別。
    └── index.ts
```

### 前端結構注意事項：

{ 解釋結構背後的任何特定慣例或理由。例如，「如果組件不是全域可重複使用的，則將其與其功能共置以提高模組化程度。」AI Agent 必須嚴格遵守此定義的結構。新檔案必須根據這些描述放置在適當的目錄中。 }

## 組件拆解與實作細節

{ 本節概述定義 UI 組件的慣例和範本。大多數特定功能組件的詳細規格將隨著 user stories 的實作而產生。AI Agent 在識別出要開發的新組件時，必須遵循下方的「組件規格範本」。 }

### 組件命名與組織

- **組件命名慣例:** **{例如，檔案和組件名稱使用 PascalCase：`UserProfileCard.tsx`}**。所有組件檔案都必須遵循此慣例。
- **組織:** {例如，「全域可重複使用的組件位於 `src/components/ui/` 或 `src/components/layout/`。特定功能的組件與其功能目錄共置，例如 `src/features/feature-name/components/`。請參閱詳細前端目錄結構。」}

### 組件規格範本

{ 對於從 UI/UX 規格書和設計檔案 (Figma) 中識別出的每個重要 UI 組件，都必須提供以下詳細資訊。為每個組件重複此小節。詳細程度必須足以讓 AI Agent 或開發人員以最少的歧義來實作它。 }

#### 組件：`{ComponentName}` (例如，`UserProfileCard`、`ProductDetailsView`)

- **用途:** {簡要描述此組件的功能及其在 UI 中的角色。必須清晰簡潔。}
- **來源檔案:** {例如，`src/components/user-profile/UserProfileCard.tsx`。必須是確切路徑。}
- **視覺參考:** {連結至特定的 Figma 框架/組件或 Storybook 頁面。必要。}
- **Props (屬性):**
  { 列出組件接受的每個 prop。對於每個 prop，表格中的所有欄位都必須填寫。 }
  | Prop 名稱 | 型別 | 必要? | 預設值 | 描述 |
  | :-------------- | :---------------------------------------- | :-------- | :------------ | :--------------------------------------------------------------------------------------------------------- |
  | `userId` | `string` | 是 | N/A | 要顯示的使用者 ID。必須是有效的 UUID。 |
  | `avatarUrl` | `string \| null` | 否 | `null` | 使用者頭像圖片的 URL。若提供，必須是有效的 HTTPS URL。 |
  | `onEdit` | `() => void` | 否 | N/A | 觸發編輯動作時的回呼函式。 |
  | `variant` | `\'compact\' \| \'full\'` | 否 | `\'full\'` | 控制卡片的顯示模式。 |
  | `{anotherProp}` | `{特定的原始型別、匯入的型別或內嵌的介面/型別定義}` | {是/否} | {若有} | {必須清楚說明 prop 的用途和任何限制，例如「必須是正整數」。} |
- **內部狀態 (若有):**
  { 描述組件管理的任何重要內部狀態。僅列出*不是*衍生自 props 或全域狀態的狀態。如果狀態複雜，請考慮是否應改由自訂 hook 或全域狀態解決方案管理。 }
  | 狀態變數 | 型別 | 初始值 | 描述 |
  | :-------------- | :-------- | :------------ | :----------------------------------------------------------------------------- |
  | `isLoading` | `boolean` | `false` | 追蹤組件資料是否正在載入。 |
  | `{anotherState}`| `{type}` | `{value}` | {狀態變數及其用途的描述。} |
- **關鍵 UI 元素 / 結構:**
  { 提供一個類似偽 HTML 或 JSX 的結構，表示組件的 DOM。若適用，請包含關鍵的條件渲染邏輯。**此結構決定了 AI Agent 的主要輸出。** }
  ```html
  <div>
    <!-- 主要卡片容器，具有特定類別，例如 styles.cardFull 或 styles.cardCompact，基於 variant prop -->
    <img
      src="{avatarUrl || defaultAvatar}"
      alt="使用者頭像"
      class="{styles.avatar}" />
    <h2>{userName}</h2>
    <p class="{variant === 'full' ? styles.emailFull : styles.emailCompact}">
      {userEmail}
    </p>
    {variant === 'full' && onEdit &&
    <button onClick="{onEdit}" class="{styles.editButton}">編輯</button>}
  </div>
  ```
- **處理/發出的事件:**
  - **處理:** {例如，編輯按鈕上的 `onClick` (觸發 `onEdit` prop)。}
  - **發出:** {如果組件發出 props 未涵蓋的自訂事件/回呼，請描述它們及其確切簽章。例如，`onFollow: (payload: { userId: string; followed: boolean }) => void`}
- **觸發的動作 (副作用):**
  - **狀態管理:** {例如，「從 `src/store/slices/userSlice.ts` 分派 `userSlice.actions.setUserName(newName)`。Action payload 必須與定義的 action creator 相符。」或「從本機 `useReducer` hook 呼叫 `updateUserProfileOptimistic(newData)`。」}
  - **API 呼叫:** {指定呼叫「API 互動層」中的哪個服務/函式。例如，「在掛載時從 `src/services/userService.ts` 呼叫 `userService.fetchUser(userId)`。請求 payload：`{ userId }`。成功回應會填入內部狀態 `userData`。錯誤回應會分派 `uiSlice.actions.showErrorToast({ message: '載入使用者詳細資料失敗' })`。」}
- **樣式注意事項:**
  - {MUST reference specific Design System component names (e.g., "Uses `<Button variant='primary'>` from UI library") OR specify Tailwind CSS classes / CSS module class names to be applied (e.g., "Container uses `p-4 bg-white rounded-lg shadow-md`. Title uses `text-xl font-semibold`.") OR specify SCSS custom component classes to be applied (e.g., "Container uses `@apply p-4 bg-white rounded-lg shadow-md`. Title uses `@apply text-xl font-semibold`."). Any dynamic styling logic based on props or state MUST be described. If Tailwind CSS is used, list primary utility classes or `@apply` directives for custom component classes. AI Agent should prioritize direct utility class usage for simple cases and propose reusable component classes/React components for complex styling patterns.}
- **無障礙注意事項:**
  - {MUST list specific ARIA attributes and their values (e.g., `aria-label="使用者個人資料卡片"`, `role="article"`), required keyboard navigation behavior (e.g., "Tab navigates to avatar, name, email, then edit button. Edit button is focusable and activated by Enter/Space."), and any focus management requirements (e.g., "If this component opens a modal, focus MUST be trapped inside. On modal close, focus returns to the trigger element.").}

---

_為每個重要組件重複上述範本。_

---

## 深入狀態管理

{ 本節擴展了狀態管理策略。**有關狀態管理解決方案的最終選擇，請參閱主要架構文件。** }

- **選擇的解決方案:** {例如，Redux Toolkit、Zustand、Vuex、NgRx - 如主要架構文件中所定義。}
- **狀態位置決策指南:**
  - **全域狀態 (例如，Redux/Zustand):** 跨多個不相關組件共用的資料；跨路由持續存在的資料；透過 reducers/thunks 管理的複雜狀態邏輯。**必須用於 session 資料、使用者偏好設定、應用程式範圍的通知。**
  - **React Context API:** 主要在特定組件子樹中向下傳遞的狀態 (例如，主題、表單 context)。與全域狀態相比，狀態更簡單，更新更少。**必須用於不適合 prop drilling 但全域不需要的本地化狀態。**
  - **本機組件狀態 (`useState`, `useReducer`):** UI 特定狀態，在組件或其直接子組件之外不需要 (例如，表單輸入值、下拉式選單開啟/關閉狀態)。**除非符合 Context 或全域狀態的標準，否則必須是預設選擇。**

### Store 結構 / Slices

{ 描述組織全域狀態的慣例 (例如，「每個需要全域狀態的主要功能都將擁有其自己的 Redux slice，位於 `src/features/[featureName]/store.ts`」)。 }

- **核心 Slice 範例 (例如，`src/store/slices/sessionSlice.ts` 中的 `sessionSlice`):**
  - **用途:** {管理使用者 session、驗證狀態和全域可存取的基本使用者個人資料資訊。}
  - **狀態結構 (介面/型別):**
    ```typescript
    interface SessionState {
      currentUser: {
        id: string;
        name: string;
        email: string;
        roles: string[];
      } | null;
      isAuthenticated: boolean;
      token: string | null;
      status: "idle" | "loading" | "succeeded" | "failed";
      error: string | null;
    }
    ```
  - **關鍵 Reducers/Actions (在 `createSlice` 內):** {簡要列出主要的同步 actions，例如 `setCurrentUser`、`clearSession`、`setAuthStatus`、`setAuthError`。}
  - **Async Thunks (若有):** {列出關鍵的 async thunks，例如 `loginUserThunk`、`fetchUserProfileThunk`。}
  - **Selectors (使用 `createSelector` 進行 memoized):** {列出關鍵 selectors，例如 `selectCurrentUser`、`selectIsAuthenticated`。}
- **功能 Slice 範本 (例如，`src/features/{featureName}/store.ts` 中的 `{featureName}Slice`):**
  - **用途:** {當新功能需要其自己的狀態 slice 時填寫。}
  - **狀態結構 (介面/型別):** {由功能定義。}
  - **關鍵 Reducers/Actions (在 `createSlice` 內):** {由功能定義。}
  - **Async Thunks (若有，使用 `createAsyncThunk` 定義):** {由功能定義。}
  - **Selectors (使用 `createSelector` 進行 memoized):** {由功能定義。}
  - **匯出:** {所有 actions 和 selectors 都必須匯出。}

### 關鍵 Selectors

{ 列出任何核心、預先定義 slices 的重要 selectors。對於新興的功能 slices，selectors 將與 slice 一起定義。**所有衍生資料或組合多個狀態片段的 selectors 都必須使用 Reselect 的 `createSelector` (或其他狀態函式庫的等效功能) 進行 memoization。** }

- **`selectCurrentUser` (來自 `sessionSlice`):** {傳回 `currentUser` 物件。}
- **`selectIsAuthenticated` (來自 `sessionSlice`):** {傳回 `isAuthenticated` 布林值。}
- **`selectAuthToken` (來自 `sessionSlice`):** {傳回 `sessionSlice` 中的 `token`。}

### 關鍵 Actions / Reducers / Thunks

{ 詳細說明核心、預先定義 slices 的更複雜 actions，尤其是非同步 thunks 或 sagas。每個 thunk 都必須清楚定義其用途、參數、進行的 API 呼叫 (參考 API 互動層)，以及如何在 pending、fulfilled 和 rejected 狀態下更新狀態。 }

- **核心 Action/Thunk 範例：`authenticateUser(credentials: AuthCredentials)` (在 `sessionSlice.ts` 中):**
  - **用途:** {透過呼叫驗證 API 並更新 `sessionSlice` 來處理使用者登入。}
  - **參數:** `credentials: { email: string; password: string }`
  - **Dispatch 流程 (使用 Redux Toolkit `createAsyncThunk`):**
    1. 在 `pending` 時：分派 `sessionSlice.actions.setAuthStatus('loading')`。
    2. 呼叫 `authService.login(credentials)` (來自 `src/services/authService.ts`)。
    3. 在 `fulfilled` (成功) 時：分派 `sessionSlice.actions.setCurrentUser(response.data.user)`、`sessionSlice.actions.setToken(response.data.token)`、`sessionSlice.actions.setAuthStatus('succeeded')`。
    4. 在 `rejected` (錯誤) 時：分派 `sessionSlice.actions.setAuthError(error.message)`、`sessionSlice.actions.setAuthStatus('failed')`。
- **功能 Action/Thunk 範本：`{featureActionName}` (在 `{featureName}Slice.ts` 中):**
  - **用途:** {用於特定功能的非同步操作。}
  - **參數:** {定義具有型別的特定參數。}
  - **Dispatch 流程 (使用 `createAsyncThunk`):** {由功能定義，遵循類似的 pending、fulfilled、rejected 狀態模式，包括 API 呼叫和狀態更新。}

## API 互動層

{ 描述前端如何與主要架構文件中定義的後端 API 進行通訊。 }

### Client/Service 結構

- **HTTP Client 設定:** {例如，`src/services/apiClient.ts` 中的 Axios 實例。**必須**包含：基礎 URL (來自環境變數 `NEXT_PUBLIC_API_URL` 或等效變數)、預設標頭 (例如，`Content-Type: 'application/json'`)、用於自動注入驗證 token (來自狀態管理，例如 `sessionSlice.token`) 和標準化錯誤處理/正規化 (見下文) 的攔截器。}
- **Service 定義 (範例):**
  - **`userService.ts` (在 `src/services/userService.ts` 中):**
    - **用途:** {處理所有與使用者相關的 API 互動。}
    - **函式:** 每個 service 函式都必須具有明確的參數型別、傳回型別 (例如，`Promise<User>`)、解釋用途、參數、傳回值和任何特定錯誤處理的 JSDoc/TSDoc。它必須使用正確的端點、方法和 payload 呼叫已設定的 HTTP client (`apiClient`)。
      - `fetchUser(userId: string): Promise<User>`
      - `updateUserProfile(userId: string, data: UserProfileUpdateDto): Promise<User>`
  - **`productService.ts` (在 `src/services/productService.ts` 中):**
    - **用途:** {...}
    - **函式:** {...}

### 錯誤處理與重試 (前端)

- **全域錯誤處理:** {如何全域攔截 API 錯誤？(例如，透過 `apiClient.ts` 中的 Axios 回應攔截器)。它們如何呈現/記錄？(例如，分派 `uiSlice.actions.showGlobalErrorBanner({ message: error.message })`，將詳細錯誤記錄到主控台/監控服務)。是否有全域錯誤狀態？(例如，`uiSlice.error`)。}
- **特定錯誤處理:** {組件可以本機處理特定的 API 錯誤，以提供更具上下文的回饋 (例如，在表單欄位上顯示內嵌訊息：「無效的電子郵件地址」)。如果偏離全域處理，則必須在組件的規格中記錄。}
- **重試邏輯:** {是否實作了客戶端重試邏輯 (例如，使用 `axios-retry` 搭配 `apiClient`)？如果是，請指定設定：最大重試次數 (例如，3)、重試條件 (例如，網路錯誤、5xx 伺服器錯誤)、重試延遲 (例如，指數退避)。**僅適用於冪等請求 (GET、PUT、DELETE)。**}

## 路由策略

{ 詳細說明前端應用程式中如何處理導覽和路由。 }

- **路由函式庫:** {例如，React Router、Next.js App Router、Vue Router、Angular Router。根據主要架構文件。}

### 路由定義

{ 列出應用程式的主要路由以及為每個路由呈現的主要組件/頁面。 }

| 路徑模式               | 組件/頁面 (`src/app/...` 或 `src/pages/...`)          | 保護                                      | 注意事項                            |
| :--------------------- | :---------------------------------------------------- | :---------------------------------------- | :---------------------------------- |
| `/`                    | `app/page.tsx` 或 `pages/HomePage.tsx`                | `Public`                                  |                                     |
| `/login`               | `app/login/page.tsx` 或 `pages/LoginPage.tsx`         | `Public` (若已驗證則重導向)               | 若已驗證，則重導向至 `/dashboard`。 |
| `/dashboard`           | `app/dashboard/page.tsx` 或 `pages/DashboardPage.tsx` | `Authenticated`                           |                                     |
| `/products`            | `app/products/page.tsx`                               | `Public`                                  |                                     |
| `/products/:productId` | `app/products/[productId]/page.tsx`                   | `Public`                                  | 參數：`productId` (string)          |
| `/settings/profile`    | `app/settings/profile/page.tsx`                       | `Authenticated`, `Role:[USER]`            | 基於角色的保護範例。                |
| `{anotherRoute}`       | `{ComponentPath}`                                     | `{Public/Authenticated/Role:[ROLE_NAME]}` | {注意事項、參數名稱和型別}          |

### 路由守衛 / 保護

- **驗證守衛:** {描述如何根據驗證狀態保護路由。**指定確切的 HOC、hook、佈局或中介軟體機制及其位置** (例如，`src/guards/AuthGuard.tsx`，或 `middleware.ts` 中的 Next.js 中介軟體)。邏輯必須使用來自 `sessionSlice` (或等效) 的驗證狀態。嘗試存取受保護路由的未驗證使用者必須被重導向至 `/login` (或指定的登入路徑)。}
- **授權守衛 (若適用):** {描述如何根據使用者角色或權限保護路由。**指定確切的機制**，類似於驗證守衛。未經授權的使用者 (已驗證但缺乏權限) 必須顯示「禁止存取」頁面或重導向至安全頁面。}

## 建置、打包與部署

{ 與前端建置和部署流程相關的詳細資訊，補充主要架構文件中的「基礎架構與部署概觀」。 }

### 建置流程與腳本

- **關鍵建置腳本 (來自 `package.json`):** {例如，`"build": "next build"`。它們做什麼？指向 `package.json` 腳本。`"dev": "next dev"`、`"start": "next start"`。}。**AI Agent 不得產生硬式編碼環境特定值的程式碼。所有此類值都必須透過定義的環境設定機制存取。**指定確切的檔案和存取方法。
- **環境設定管理:** {如何為不同環境 (開發、預備、生產) 管理 `process.env.NEXT_PUBLIC_API_URL` (或等效的如 `import.meta.env.VITE_API_URL`)？(例如，Next.js/Vite 的 `.env`、`.env.development`、`.env.production` 檔案；透過 CI 變數在建置時注入)。指定確切的檔案和存取方法。}

### 關鍵打包優化

- **程式碼分割:** {如何實作/確保？(例如，「Next.js/Vite 會自動處理基於路由的程式碼分割。對於組件級程式碼分割，非關鍵的大型組件/函式庫必須使用動態匯入 `React.lazy(() => import('./MyComponent'))` 或 `import('./heavy-module')`。」)}
- **Tree Shaking:** {如何實作/確保？(例如，「由現代建置工具如 Webpack/Rollup (Next.js/Vite 使用) 在使用 ES Modules 時確保。避免在共用函式庫中進行有副作用的匯入。」)}
- **延遲載入 (組件、圖片等):** {延遲載入的策略。(例如，「組件：`React.lazy` 搭配 `Suspense`。圖片：使用框架特定的圖片組件如 `next/image` (預設處理延遲載入)，或標準 `<img>` 標籤的 `loading='lazy'` 屬性。」)}
- **最小化與壓縮:** {由建置工具處理 (例如，Webpack/Terser、Vite/esbuild)？指定是否需要任何特定設定。壓縮 (例如，Gzip、Brotli) 通常由託管平台/CDN 處理。}

### 部署至 CDN/託管

- **目標平台:** {例如，Vercel、Netlify、AWS S3/CloudFront、Azure Static Web Apps。根據主要架構文件。}
- **部署觸發器:** {例如，透過 GitHub Actions 將 Git 推送至 `main` 分支 (參考主要 CI/CD 管線)。}
- **資產快取策略:** {靜態資產如何快取？(例如，「不可變資產 (具有內容雜湊的 JS/CSS 打包檔) 具有 `Cache-Control: public, max-age=31536000, immutable`。HTML 檔案具有 `Cache-Control: no-cache` 或較短的 max-age (例如，`public, max-age=0, must-revalidate`) 以確保使用者取得最新的進入點。透過 {託管平台設定 / `next.config.js` 標頭 / CDN 規則} 設定。}

## 前端測試策略

{ 本節詳細說明主要架構文件中的「測試策略」，著重於前端特定方面。**有關測試工具的最終選擇，請參閱主要架構文件。** }

- **主要整體測試策略連結:** {參考主要的 `docs/architecture.md#overall-testing-strategy` 或等效文件。}

### 組件測試

- **範圍:** {在隔離環境中測試個別 UI 組件 (類似於組件的單元測試)。}
- **工具:** {例如，React Testing Library 搭配 Jest、Vitest、Vue Test Utils、Angular Testing Utilities。根據主要架構文件。}
- **重點:** {使用各種 props 進行渲染、使用者互動 (點擊、使用 `fireEvent` 或 `userEvent` 進行輸入變更)、事件發出、基本內部狀態變更。**快照測試必須謹慎使用，並有明確的理由 (例如，用於非常穩定、純展示性且具有複雜 DOM 結構的組件)；優先使用明確的斷言。**}
- **位置:** {例如，與組件共置的 `*.test.tsx` 或 `*.spec.tsx`，或在 `__tests__` 子目錄中。}

### 功能/流程測試 (UI 整合)

- **範圍:** {測試多個組件如何互動以完成頁面內的小型使用者流程或功能，可能模擬 API 呼叫或全域狀態管理。例如，測試功能內的完整表單提交，包括驗證和與模擬服務層的互動。}
- **工具:** {與組件測試相同 (例如，React Testing Library 搭配 Jest/Vitest)，但設定更複雜，涉及路由、狀態、API 呼叫的模擬提供者。}
- **重點:** {組件之間的資料流、基於互動的條件渲染、功能內的導覽、與模擬服務/狀態的整合。}

### 端對端 UI 測試工具與範圍

- **工具:** {重申主要測試策略，例如 Playwright、Cypress、Selenium。}
- **範圍 (前端重點):** {定義 3-5 個必須由 E2E UI 測試從 UI 角度涵蓋的關鍵使用者旅程，例如，「使用者註冊和登入流程」、「將商品加入購物車並進入結帳頁面摘要」、「提交複雜的多步驟表單並驗證成功 UI 狀態和資料持久性 (透過 API 模擬或測試後端)。」}
- **UI 的測試資料管理:** {如何為 UI E2E 測試植入或模擬一致的測試資料？(例如，API 模擬層如 MSW、後端植入腳本、專用測試帳戶)。}

## 無障礙 (AX) 實作細節

{ 根據 UI/UX 規格書中的 AX 需求，詳細說明如何技術性地實作這些需求。 }

- **語義化 HTML:** {強調使用正確的 HTML5 元素。**AI Agent 必須優先使用語義化元素 (例如，`<nav>`、`<button>`、`<article>`)，而不是在存在具有正確語義的原生元素的情況下使用帶有 ARIA roles 的通用 `<div>`/`<span>`。**}
- **ARIA 實作:** {指定常見的自訂組件及其必要的 ARIA 模式 (例如，「自訂下拉式選單必須遵循 ARIA Combobox 模式，包括 `aria-expanded`、`aria-controls`、`role='combobox'` 等。自訂頁籤必須遵循 ARIA Tabbed Interface 模式。」)。連結至 ARIA Authoring Practices Guide (APG) 以供參考。}
- **鍵盤導覽:** {確保所有互動元素都可透過鍵盤聚焦和操作。焦點順序必須合乎邏輯。自訂組件必須根據 ARIA APG 實作鍵盤互動模式 (例如，用於單選按鈕群組/滑桿的箭頭鍵)。\*\*}
- **焦點管理:** {在強制回應視窗、動態內容變更、路由轉換中如何管理焦點？(例如，「強制回應視窗必須限制焦點。開啟強制回應視窗時，焦點移至第一個可聚焦元素或強制回應視窗容器。關閉時，焦點返回觸發元素。路由變更應將焦點移至新頁面的主要內容區域或 H1。」)}
- **AX 測試工具:** {例如，Axe DevTools 瀏覽器擴充功能、Lighthouse 無障礙稽核。**自動化 Axe 掃描 (例如，使用 `jest-axe` 進行組件測試，或使用 Playwright/Cypress Axe 整合進行 E2E 測試) 必須整合到 CI 管線中，並在出現新的 WCAG AA (或指定等級) 違規時使建置失敗。**手動測試程序：{列出關鍵的手動檢查，例如，所有互動元素的僅鍵盤導覽、關鍵使用者流程的螢幕閱讀器測試 (例如，NVDA/JAWS/VoiceOver)。}}

## 效能考量

{ 強調前端特定的效能優化策略。 }

- **圖片優化:** {格式 (例如，WebP)、響應式圖片 (`<picture>`、`srcset`)、延遲載入。}
  - 實作要求：{例如，「所有圖片都必須使用 Next.js (或等效的框架特定優化器) 的 `<Image>` 組件。圖示使用 SVG。在支援的情況下優先使用 WebP 格式。」}
- **程式碼分割與延遲載入 (若需要，重申建置部分):** {它如何影響感知效能。}
  - 實作要求：{例如，「Next.js 會自動處理基於路由的程式碼分割。必須使用動態匯入 `import()` 進行組件級延遲載入。」}
- **最小化重新渲染:** {技術如 `React.memo`、`shouldComponentUpdate`、優化 selectors。}
  - 實作要求：{例如，「對於使用相同 props 頻繁渲染的組件，必須使用 `React.memo`。全域狀態的 selectors 必須進行 memoized (例如，使用 Reselect)。避免在 render 方法中直接將新的物件/陣列常值或內嵌函式作為 props 傳遞，因為這可能導致不必要的重新渲染。」}
- **Debouncing/Throttling:** {用於事件處理常式，如搜尋輸入或視窗調整大小。}
  - 實作要求：{例如，「為指定的事件處理常式使用 `lodash.debounce` 或 `lodash.throttle` 之類的工具。定義 debounce/throttle 等待時間。」}
- **虛擬化:** {用於長列表或大型資料集 (例如，React Virtualized、TanStack Virtual)。}
  - 實作要求：{例如，「如果觀察到效能下降，則必須用於任何渲染超過 {N，例如 100} 個項目的列表。」}
- **快取策略 (客戶端):** {使用瀏覽器快取、用於 PWA 功能的 service workers (若適用)。}
  - 實作要求：{例如，「設定 service worker (若是 PWA) 以快取應用程式殼層和關鍵靜態資產。利用 HTTP 快取標頭處理部署部分中定義的其他資產。」}
- **效能監控工具:** {例如，Lighthouse、WebPageTest、瀏覽器開發者工具效能分頁。指定哪些是主要的，以及 CI 中的任何自動化檢查。}

## 國際化 (i18n) 與本地化 (l10n) 策略

{本節定義了在適用情況下支援多種語言和區域差異的策略。如果不適用，請註明「國際化目前不是此專案的需求。」}

- **需求等級:** {例如，不需要、特定語言需要 [列出它們]、為未來擴展完全國際化。}
- **選擇的 i18n 函式庫/框架:** {例如，`react-i18next`、`vue-i18n`、`ngx-translate`、框架原生解決方案如 Next.js i18n 路由。指定確切的函式庫/機制。}
- **翻譯檔案結構與格式:** {例如，每個功能每種語言的 JSON 檔案 (`src/features/{featureName}/locales/{lang}.json`)，或全域檔案 (`public/locales/{lang}.json`)。定義確切的路徑和格式 (例如，扁平 JSON、巢狀 JSON)。}
- **翻譯鍵命名慣例:** {例如，`featureName.componentName.elementText`、`common.submitButton`。必須是清晰、一致且有文件記錄的模式。}
- **新增可翻譯字串的流程:** {例如，「AI Agent 必須將新鍵新增至預設語言檔案 (例如，`en.json`)，並使用 i18n 函式庫的函式/組件 (例如，`<Trans>` 組件、`t()` 函式) 來渲染文字。不得在執行時期以阻止靜態分析的方式動態建構鍵。」}
- **處理複數:** {指定方法/語法，例如，透過選擇的函式庫使用 ICU 訊息格式 (例如，`t('key', { count: N })`)。}
- **日期、時間和數字格式化:** {指定 i18n 函式庫是否處理此問題，或者是否應為每個地區設定使用其他函式庫 (例如，具有地區設定支援的 `date-fns-tz`、直接使用 `Intl` API) 和特定格式/樣式。}
- **預設語言:** {例如，`en-US`}
- **語言切換機制 (若適用):** {使用者如何變更語言並使其持續存在？例如，「透過語言選擇器組件更新全域狀態/cookie，並可能更改 URL 路由。」}

## 功能旗標管理

{本節概述如何管理有條件啟用的功能。如果不適用，請註明「功能旗標目前不是此專案的主要架構考量。」}

- **需求等級:** {例如，不需要、用於特定推出、開發工作流程的核心部分。}
- **選擇的功能旗標系統/函式庫:** {例如，LaunchDarkly、Unleash、Flagsmith、使用環境變數或設定服務的自訂解決方案。指定確切的工具/方法。}
- **在程式碼中存取旗標:** {例如，「透過自訂 hook `useFeatureFlag('flag-name'): boolean` 或服務 `featureFlagService.isOn('flag-name')`。指定服務/提供者的確切介面、位置和初始化。」}
- **旗標命名慣例:** {例如，`[SCOPE]_[FEATURE_NAME]_[TARGET_GROUP_OR_TYPE]`，例如 `CHECKOUT_NEW_PAYMENT_GATEWAY_ROLLOUT`、`USER_PROFILE_BETA_AVATAR_UPLOAD`。必須有文件記錄並一致應用。}
- **標記功能的程式碼結構:** {例如，「使用條件渲染 (`{isFeatureEnabled && <NewComponent />}`)。對於較大的功能，有條件地匯入組件 (`React.lazy` 搭配旗標檢查) 或路由。避免在共用組件深處使用複雜的分支邏輯；優先在較高層級標記。」}
- **程式碼清理策略 (旗標停用後):** {例如，「一旦旗標完全推出 (100% 使用者) 並被視為永久性，或完全移除，所有條件邏輯、舊程式碼路徑和旗標本身都必須在 {N，例如 2} 個 sprints 內從程式碼庫中移除。這是強制性的技術債項目。」}
- **測試標記功能:** {如何測試不同的旗標變體？例如，「QA 團隊使用偵錯面板來切換旗標。自動化 E2E 測試使用特定的旗標設定執行。」}

## 前端安全性考量

{本節強調強制性的前端特定安全性實務，補充主要架構文件。AI Agent 必須遵守這些準則。}

- **跨網站指令碼 (XSS) 防護:**
  - 框架依賴：{例如，「必須依賴 React 的 JSX 自動逸出功能來渲染動態內容。除非內容經過明確清理，否則必須避免使用 Vue 的 `v-html`。」}
  - 明確清理：{如果無法避免直接 DOM 操作 (強烈不建議)，請使用 {特定的清理函式庫/函式，如 DOMPurify}。指定其設定。}
  - 內容安全政策 (CSP)：{是否實作 CSP？如何實作？例如，「CSP 透過後端/CDN 設定的 HTTP 標頭強制執行，如主要架構文件中所定義。如果 `unsafe-inline` 不被允許，前端可能需要確保內嵌腳本使用 nonce。」連結至 CSP 定義 (若有)。}
- **跨網站請求偽造 (CSRF) 保護 (若適用於基於 session 的驗證):**
  - 機制：{例如，「後端使用同步器 token 模式。如果 HTTP client 或表單未自動處理，前端確保在狀態變更請求中包含 token。」參考主要架構文件以取得後端詳細資訊。}
- **安全 Token 儲存與處理 (用於客戶端 token，如 JWT):**
  - 儲存機制：{**必須指定確切機制**：例如，透過狀態管理 (例如 Redux/Zustand store，在關閉分頁時清除) 在記憶體中儲存、`HttpOnly` cookies (如果後端設定它們且前端不需要讀取它們)、`sessionStorage`。**強烈不建議使用 `localStorage` 儲存 token。**}
  - Token 更新：{描述客戶端參與，例如，「`apiClient.ts` 中的攔截器處理 401 錯誤以觸發 token 更新端點。」}
- **第三方腳本安全性:**
  - 政策：{例如，「所有第三方腳本 (分析、廣告、小工具) 都必須經過必要性和安全性審查。非同步載入腳本 (`async/defer`)。」}
  - 子資源完整性 (SRI)：{例如，「對於從 CDN 載入的所有外部腳本和樣式表 (如果資源穩定)，都必須使用 SRI 雜湊。」}
- **客戶端資料驗證:**
  - 用途：{例如，「客戶端驗證僅用於改善 UX (即時回饋)。**所有關鍵資料驗證都必須在伺服器端進行** (如主要架構文件中所定義)。」}
  - 實作：{例如，「使用 {表單函式庫名稱，如 Formik/React Hook Form} 進行表單驗證。規則應在適當情況下反映伺服器端驗證。」}
- **防止點擊劫持:**
  - 機制：{例如，「主要防禦是 `X-Frame-Options` 或 `frame-ancestors` CSP 指令，由後端/CDN 設定。前端程式碼不應依賴 frame-busting 腳本。」}
- **API 金鑰洩漏 (用於客戶端使用的服務):**
  - 限制：{例如，「用於 Google 地圖 (客戶端 JS SDK) 等服務的 API 金鑰必須透過服務提供者的主控台進行限制 (例如，HTTP 參照位址、IP 位址、API 限制)。」}
  - 後端代理：{例如，「對於需要更高保密性或涉及敏感操作的金鑰，必須建立後端代理端點；前端呼叫代理，而不是直接呼叫第三方服務。」}
- **安全通訊 (HTTPS):**
  - 要求：{例如，「與後端 API 的所有通訊都必須使用 HTTPS。禁止混合內容 (HTTPS 頁面上的 HTTP 資產)。」}
- **相依性弱點:**
  - 流程：{例如，「在 CI 中執行 `npm audit --audit-level=high` (或等效指令)。高/嚴重弱點必須在部署前解決。監控 Dependabot/Snyk 警示。」}

## 瀏覽器支援與漸進式增強

{本節定義目標瀏覽器以及應用程式在功能較弱或非標準環境中的行為方式。}

- **目標瀏覽器:** {例如，「Chrome、Firefox、Safari、Edge 的最新 2 個穩定版本。如果專案限制需要，可以列出特定版本。不支援 Internet Explorer (任何版本)。」必須明確。}
- **Polyfill 策略:**
  - 機制：{例如，「在應用程式進入點匯入 `core-js@3`。Babel `preset-env` 設定了上述瀏覽器目標以包含必要的 polyfills。」}
  - 特定 Polyfills (若 `core-js` 之外還有其他): {列出特定功能所需的任何其他 polyfills，例如 `smoothscroll-polyfill`。}
- **JavaScript 需求與漸進式增強:**
  - 基準：{例如，「核心應用程式功能需要在瀏覽器中啟用 JavaScript。」或「關鍵內容 (例如，文章、產品資訊) 和主要導覽必須在沒有 JavaScript 的情況下可存取和閱讀。互動功能和增強功能透過 JavaScript 分層疊加 (漸進式增強方法)。」指定選擇的方法。}
  - 無 JS 體驗 (若採用漸進式增強): {描述在沒有 JS 的情況下哪些功能可用。例如，「使用者可以檢視頁面和導覽。表單可能無法提交或將使用標準 HTML 提交。」}
- **CSS 相容性與備援:**
  - 工具：{例如，「使用 Autoprefixer (透過 PostCSS) 設定目標瀏覽器列表以新增供應商前綴。」}
  - 功能使用：{例如，「避免使用目標瀏覽器支援率低於 90% 的 CSS 功能，除非明確定義並測試了優雅降級或備援 (例如，使用 `@supports` 查詢)。」}
- **無障礙備援:** {考慮如果支援矩陣中較舊的輔助技術不支援某些 ARIA 版本或進階無障礙功能，功能將如何運作。}

## 變更日誌

| 變更 | 日期 | 版本 | 描述 | 作者 |
| ---- | ---- | ---- | ---- | ---- |

==================== END: front-end-architecture-tmpl ====================

==================== START: front-end-spec-tmpl ====================

# {Project Name} UI/UX 規格書

## 簡介

{說明目的 - 定義專案使用者介面的使用者體驗目標、資訊架構、使用者流程和視覺設計規格。}

- **主要設計檔案連結:** {例如，Figma、Sketch、Adobe XD URL}
- **已部署 Storybook / Design System 連結:** {URL，若適用}

## 整體 UX 目標與原則

- **目標使用者畫像:** {參考畫像或簡要描述關鍵使用者類型及其目標。}
- **可用性目標:** {例如，易學性、使用效率、錯誤預防。}
- **設計原則:** {列出 3-5 個指導 UI/UX 設計的核心原則 - 例如，「清晰勝於巧妙」、「一致性」、「提供回饋」。}

## 資訊架構 (IA)

- **網站地圖 / 畫面清單:**
  ```mermaid
  graph TD
      A[Homepage] --> B(Dashboard);
      A --> C{Settings};
      B --> D[View Details];
      C --> E[Profile Settings];
      C --> F[Notification Settings];
  ```
  _(或提供所有畫面/頁面的列表)_
- **導覽結構:** {描述主要導覽 (例如，頂端列、側邊欄)、次要導覽、麵包屑等。}

## 使用者流程

{詳細說明關鍵使用者任務。使用圖表或描述。}

### {使用者流程名稱，例如使用者登入}

- **目標:** {使用者想要達成的目標。}
- **步驟 / 圖表:**
  ```mermaid
  graph TD
      Start --> EnterCredentials[輸入電子郵件/密碼];
      EnterCredentials --> ClickLogin[點擊登入按鈕];
      ClickLogin --> CheckAuth{驗證成功?};
      CheckAuth -- Yes --> Dashboard;
      CheckAuth -- No --> ShowError[顯示錯誤訊息];
      ShowError --> EnterCredentials;
  ```
  _(或：連結至 Figma/Miro 中的特定流程圖)_

### {另一個使用者流程名稱}

{...}

## 線框稿與模型

{參考上方的設計檔案主連結。可選擇嵌入關鍵模型或描述主要畫面佈局。}

- **畫面 / 視圖名稱 1:** {佈局和關鍵元素的描述。連結至特定的 Figma 框架/頁面。}
- **畫面 / 視圖名稱 2:** {...}

## 組件庫 / Design System 參考

## 品牌與風格指南參考

{連結至主要來源或在此定義關鍵元素。}

- **調色盤:** {主要、次要、強調、回饋顏色 (十六進位碼)。}
- **排版:** {標題、內文等的字型家族、大小、粗細。}
- **圖示:** {連結至圖示集、使用注意事項。}
- **間距與網格:** {定義邊界、內距、網格系統規則。}

## 無障礙 (AX) 需求

- **目標合規性:** {例如，WCAG 2.1 AA}
- **特定需求:** {鍵盤導覽模式、複雜組件的 ARIA 地標/屬性、色彩對比度最小值。}

## 響應式設計

- **中斷點:** {定義行動裝置、平板電腦、桌上型電腦等的像素值。}
- **適應策略:** {描述佈局和組件如何在不同中斷點之間適應。參考設計。}

## 變更日誌

| 變更 | 日期 | 版本 | 描述 | 作者 |
| ---- | ---- | ---- | ---- | ---- |

==================== END: front-end-spec-tmpl ====================

==================== START: prd-tmpl ====================

# {Project Name} 產品需求文件 (PRD)

## 目標、目的與背景

這部分應主要來自使用者或提供的簡報，但可視需要要求澄清。

## 功能需求 (MVP)

此時您應該已經有很好的想法，但仍需澄清、建議問題並解釋以確保這些是正確的。

## 非功能需求 (MVP)

此時您應該已經有很好的想法，但仍需澄清、建議問題並解釋以確保這些是正確的。

## 使用者互動與設計目標

{
如果產品包含使用者介面 (UI)，本節將擷取產品經理對使用者體驗 (UX) 的高階願景和目標。此資訊將作為設計架構師的重要起點和簡報。

考慮並從使用者處獲取以下資訊：

- **整體願景與體驗：** 期望的外觀和感覺是什麼（例如，「現代簡約」、「友善易用」、「資料密集且專業」）？使用者應該擁有什麼樣的體驗？
- **關鍵互動模式：** 使用者與核心功能的互動是否有特定的方式（例如，「X 的拖放介面」、「Y 的精靈式設定」、「Z 的即時儀表板」）？
- **核心畫面/視圖 (概念性)：** 從產品角度來看，哪些是最關鍵的畫面或視圖，以實現 MVP 的價值？（例如，「登入畫面」、「主儀表板」、「項目詳細資料頁面」、「設定頁面」）。
- **無障礙期望：** 是否有任何已知的高階無障礙目標（例如，「必須可供螢幕閱讀器使用者使用」）。
- **品牌考量 (高階)：** 是否有任何已知的品牌元素或風格指南必須納入？
- **目標裝置/平台：** （例如，「主要為網頁桌上型電腦」、「行動優先的響應式網頁應用程式」）。

本節並非旨在成為詳細的 UI 規格書，而是以產品為中心的簡報，以指導設計架構師後續的詳細工作，設計架構師將建立完整的 UI/UX 規格書文件。
}

## 技術假設

我們可以在這裡列出主要供架構師產生技術細節的資訊。這可以是我們已經知道或從使用者處獲得的任何高階技術資訊。向使用者詢問以獲得關於語言、框架、入門範本知識、函式庫、外部 API、潛在函式庫選擇等的基本概念。

- **儲存庫與服務架構：** {關鍵決策：記錄選擇的儲存庫結構（例如，Monorepo、Polyrepo）和高階服務架構（例如，Monolith、Microservices、Monorepo 內的 Serverless functions）。根據專案目標、MVP 範圍、團隊結構和可擴展性需求解釋理由。此決策直接影響技術方法並通知架構師代理。}

### 測試需求

除了單元測試之外，我們將如何驗證功能？我們是否需要手動腳本或測試、e2e、整合等... 從使用者處了解以填寫本節。

## Epic 概觀

- **Epic {#}: {標題}**
  - 目標：{簡潔的 1-2 句話，描述此 Epic 的主要目標和價值。}
  - Story {#}: 身為 {使用者/系統類型}，我想要 {執行一個動作 / 達成一個目標} 以便於 {我可以實現一個益處 / 達成一個理由}。
    - {驗收標準列表}
  - Story {#}: 身為 {使用者/系統類型}，我想要 {執行一個動作 / 達成一個目標} 以便於 {我可以實現一個益處 / 達成一個理由}。
    - {驗收標準列表}
- **Epic {#}: {標題}**
  - 目標：{簡潔的 1-2 句話，描述此 Epic 的主要目標和價值。}
  - Story {#}: 身為 {使用者/系統類型}，我想要 {執行一個動作 / 達成一個目標} 以便於 {我可以實現一個益處 / 達成一個理由}。
    - {驗收標準列表}
  - Story {#}: 身為 {使用者/系統類型}，我想要 {執行一個動作 / 達成一個目標} 以便於 {我可以實現一個益處 / 達成一個理由}。
    - {驗收標準列表}

## 關鍵參考文件

{ 本節稍後將根據先前章節分割成較小文件後建立。 }

## MVP 後超出範圍的想法

您和使用者同意超出範圍或可以從範圍中移除以保持 MVP 精簡的任何內容。考慮 PRD 的目標，以及哪些可能是額外的鍍金或附加功能，可以等到 MVP 完成並交付以評估功能和市場契合度或使用情況後再進行。

## [可選：僅適用於簡化的 PM 到開發工作流程] 核心技術決策與應用程式結構

{本節僅在 PM 在「簡化的 PM 到開發工作流程」中操作時填寫。它擷取了通常由架構師定義的基本技術基礎，從而實現更直接的開發路徑。此資訊應在初步 PRD 章節（目標、使用者等）起草後，理想情況下在詳細的 Epic/Story 定義之前或與之並行收集，並視需要更新。}

### 技術堆疊選擇

{協同定義核心技術。在適當情況下，具體說明選擇和版本。}

- **主要後端語言/框架：** {例如，Python/FastAPI、Node.js/Express、Java/Spring Boot}
- **主要前端語言/框架 (若適用)：** {例如，TypeScript/React (Next.js)、JavaScript/Vue.js}
- **資料庫：** {例如，PostgreSQL、MongoDB、AWS DynamoDB}
- **關鍵函式庫/服務 (後端)：** {例如，驗證 (JWT、OAuth provider)、ORM (SQLAlchemy)、快取 (Redis)}
- **關鍵函式庫/服務 (前端，若適用)：** {例如，UI 組件庫 (Material-UI、Tailwind CSS + Headless UI)、狀態管理 (Redux、Zustand)}
- **部署平台/環境：** {例如，AWS ECS 上的 Docker、Vercel、Netlify、Kubernetes}
- **版本控制系統：** {例如，Git 搭配 GitHub/GitLab}

### 建議的應用程式結構

{描述程式碼庫的高階組織。這可能包括簡單的文字型目錄佈局、主要模組/組件列表，以及它們如何互動的簡要說明。目標是為開發人員提供一個清晰的起點。}

範例：

```
/
├── app/                  # 主要應用程式原始碼
│   ├── api/              # 後端 API 路由和邏輯
│   │   ├── v1/
│   │   └── models.py
│   ├── web/              # 前端組件和頁面 (若為單體應用程式)
│   │   ├── components/
│   │   └── pages/
│   ├── core/             # 共用業務邏輯、工具程式
│   └── main.py           # 應用程式進入點
├── tests/                # 單元和整合測試
├── scripts/              # 工具腳本
├── Dockerfile
├── requirements.txt
└── README.md
```

- **Monorepo/Polyrepo：** {指定是否設想 Monorepo 或 Polyrepo 結構，並簡要說明原因。}
- **關鍵模組/組件與職責：**
  - {模組 1 名稱}：{其用途和關鍵職責的簡要描述}
  - {模組 2 名稱}：{其用途和關鍵職責的簡要描述}
  - ...
- **資料流概觀 (概念性)：** {簡要描述資料預期如何在主要組件之間流動，例如，前端 -> API -> 核心邏輯 -> 資料庫。}

## 變更日誌

| 變更 | 日期 | 版本 | 描述 | 作者 |
| ---- | ---- | ---- | ---- | ---- |

----- END PRD START CHECKLIST OUTPUT ------

## 清單結果報告

----- END Checklist START Design Architect `UI/UX Specification Mode` Prompt ------

----- END Design Architect `UI/UX Specification Mode` Prompt START Architect Prompt ------

## 初始架構師提示

根據我們對 {Product Name} 的討論和需求分析，我編制了以下技術指南，以告知您的架構分析和決策，從而啟動架構建立模式：

### 技術基礎架構

- **儲存庫與服務架構決策：** {重申在「技術假設」中所做的決策，例如，Monorepo 搭配 Next.js 前端和 Python FastAPI 後端服務在同一個儲存庫中；或 Polyrepo 搭配獨立的前端 (Next.js) 和後端 (Spring Boot Microservices) 儲存庫。}
- **入門專案/範本：** {關於應使用的任何入門專案、範本或現有程式碼庫的資訊}
- **託管/雲端供應商：** {指定的雲端平台 (AWS、Azure、GCP 等) 或託管需求}
- **前端平台：** {框架/函式庫偏好或需求 (React、Angular、Vue 等)}
- **後端平台：** {框架/語言偏好或需求 (Node.js、Python/Django 等)}
- **資料庫需求：** {關聯式、NoSQL、特定產品或服務偏好}

### 技術限制

- {列出任何影響架構決策的技術限制}
- {包含任何強制性的技術、服務或平台}
- {注意任何具有特定技術影響的整合需求}

### 部署考量

- {部署頻率期望}
- {CI/CD 需求}
- {環境需求 (本機、開發、預備、生產)}

### 本機開發與測試需求

{僅當使用者表示這些功能很重要時才包含此部分。如果根據使用者偏好不適用，您可以移除此部分。}

- {本機開發環境的需求}
- {對命令列測試功能的期望}
- {跨不同環境測試的需求}
- {應提供的工具腳本或工具}
- {任何特定的組件可測試性需求}

### 其他技術考量

- {具有技術影響的安全性需求}
- {具有架構影響的可擴展性需求}
- {架構師應考慮的任何其他技術背景}

----- END Architect Prompt -----

==================== END: prd-tmpl ====================

==================== START: project-brief-tmpl ====================

# 專案簡報：{Project Name}

## 簡介 / 問題陳述

{描述核心概念、正在解決的問題或正在處理的機會。為什麼需要這個專案？}

## 願景與目標

- **願景：** {描述此專案期望的高階未來狀態或影響。}
- **主要目標：** {列出 2-5 個針對最小可行產品 (MVP) 的具體、可衡量、可實現、相關且有時限 (SMART) 的目標。}
  - 目標 1：...
  - 目標 2：...
- **成功指標 (初步想法)：** {我們將如何衡量專案/MVP 是否成功？列出潛在的 KPI。}

## 目標受眾 / 使用者

{描述此產品/系統的主要使用者。他們是誰？他們與此專案相關的關鍵特徵或需求是什麼？}

## 關鍵功能 / 範圍 (MVP 的高階想法)

{列出為 MVP 設想的核心功能。保持高階；詳細資訊將記錄在 PRD/Epics 中。}

- 功能想法 1：...
- 功能想法 2：...
- 功能想法 N：...

## MVP 後功能 / 範圍與想法

{列出設想為 MVP 後潛在的核心功能。保持高階；詳細資訊將記錄在 PRD/Epics/架構中。}

- 功能想法 1：...
- 功能想法 2：...
- 功能想法 N：...

## 已知技術限制或偏好

- **限制：** {列出任何已知的限制以及技術要求或偏好 - 例如，預算、時程、特定技術要求、必要的整合、合規性需求。}
- **初步架構偏好 (若有)：** {擷取關於儲存庫結構 (例如，monorepo、polyrepo) 和整體服務架構 (例如，monolith、microservices、serverless components) 的任何早期想法或強烈偏好。這不是最終決策點，而是用於初步了解。}
- **風險：** {識別潛在風險 - 例如，技術挑戰、資源可用性、市場接受度、相依性。}
- **使用者偏好：** {使用者提出的任何非高階功能的特定請求，這些請求可能指導技術或函式庫的選擇，或在腦力激盪或起草 PRD 過程中出現但未包含在先前文件章節中的任何其他內容}

## 相關研究 (可選)

{連結至或總結任何初步研究的發現 (例如，`deep-research-report-BA.md`)。}

## PM 提示

此專案簡報提供了 {Project Name} 的完整背景。請以「PRD 產生模式」開始，徹底檢閱簡報，以便與使用者逐節建立 PRD，並根據您的模式 1 程式設計能力，要求任何必要的澄清或建議改進。

<example_handoff_prompt>
此專案簡報提供了 Mealmate 的完整背景。請以「PRD 產生模式」開始，徹底檢閱簡報，以便與使用者逐節建立 PRD，並根據您的模式 1 程式設計能力，要求任何必要的澄清或建議改進。</example_handoff_prompt>

==================== END: project-brief-tmpl ====================

==================== START: story-tmpl ====================

# Story {EpicNum}.{StoryNum}: {從 Epic 檔案複製的簡短標題}

## 狀態：{ 草稿 | 已核准 | 進行中 | 完成 }

## Story

- 身為 [角色]
- 我想要 [動作]
- 以便於 [益處]

## 驗收標準 (ACs)

{ 複製驗收標準編號列表 }

## 任務 / 子任務

- [ ] 任務 1 (AC：# 若適用)
  - [ ] 子任務 1.1...
- [ ] 任務 2 (AC：# 若適用)
  - [ ] 子任務 2.1...
- [ ] 任務 3 (AC：# 若適用)
  - [ ] 子任務 3.1...

## 開發技術指南 {任務/子任務未涵蓋的詳細資訊}

## Story 進度筆記

### 使用的 Agent 模型：`<Agent 模型名稱/版本>`

### 完成筆記列表

{關於實作選擇、困難或需要後續追蹤的任何筆記}

### 變更日誌

==================== END: story-tmpl ====================
