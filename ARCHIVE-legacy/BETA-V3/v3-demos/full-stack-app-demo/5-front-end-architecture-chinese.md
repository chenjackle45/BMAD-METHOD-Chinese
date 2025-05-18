# BMad DiCaster 前端架構文件

## 目錄

- [引言](#introduction)
- [整體前端理念與模式](#overall-frontend-philosophy--patterns)
- [詳細前端目錄結構](#detailed-frontend-directory-structure)
- [元件分解與實作細節](#component-breakdown--implementation-details)
  - [元件命名與組織](#component-naming--organization)
  - [元件規格範本](#template-for-component-specification)
- [狀態管理深入探討](#state-management-in-depth)
  - [選擇的解決方案](#chosen-solution)
  - [理由](#rationale)
  - [Store 結構 / Slices](#store-structure--slices)
  - [關鍵 Selectors](#key-selectors)
  - [關鍵 Actions / Reducers / Thunks](#key-actions--reducers--thunks)
- [API 互動層](#api-interaction-layer)
  - [Client/Service 結構](#clientservice-structure)
  - [錯誤處理與重試 (前端)](#error-handling--retries-frontend)
- [路由策略](#routing-strategy)
  - [路由函式庫](#routing-library)
  - [路由定義](#route-definitions)
  - [路由守衛 / 保護](#route-guards--protection)
- [建構、打包與部署](#build-bundling-and-deployment)
  - [建構流程與腳本](#build-process--scripts)
  - [關鍵打包優化](#key-bundling-optimizations)
  - [部署到 CDN/Hosting](#deployment-to-cdnhosting)
- [前端測試策略](#frontend-testing-strategy)
  - [主測試策略連結](#link-to-main-testing-strategy)
  - [元件測試](#component-testing)
  - [UI 整合/流程測試](#ui-integrationflow-testing)
  - [端到端 UI 測試工具與範圍](#end-to-end-ui-testing-tools--scope)
- [無障礙 (AX) 實作細節](#accessibility-ax-implementation-details)
- [效能考量](#performance-considerations)
- [變更日誌](#change-log)

## 引言

本文件詳細說明 BMad DiCaster 前端特定的技術架構。它補充了主要的 BMad DiCaster 架構文件和 UI/UX 規格。目標是為前端開發提供清晰的藍圖，確保一致性、可維護性，並與整體系統設計和使用者體驗目標保持一致。

- **主架構文件連結：** `docs/architecture.md` (注意：整體系統架構，包括 Monorepo/Polyrepo 決策和後端結構，將影響前端的選擇，特別是關於共享程式碼或 API 互動模式。)
- **UI/UX 規格連結：** `docs/ui-ux-spec.txt`
- **主要設計檔案連結 (Figma, Sketch 等)：** N/A (低保真線框圖描述於 `docs/ui-ux-spec.txt` 中；詳細模擬圖將在開發期間建立)
- **已部署的 Storybook / 元件展示連結 (如果適用)：** N/A (待開發)

## 整體前端理念與模式

BMad DiCaster 的前端將使用現代、高效且可維護的實踐來建構，並以 Vercel/Supabase Next.js App Router 範本作為起點。核心理念是建立一個響應式、快速載入且易於存取的使用者介面，並與「synthwave 技術發光紫色氛圍」美學保持一致。

- **框架與核心函式庫：**

  - **Next.js (最新版本，例如 14.x.x, App Router)：** 選擇它是因為其強大的全端功能、伺服器端渲染 (SSR) 和靜態網站生成 (SSG) 選項、優化的效能以及與 Vercel 的無縫整合以進行部署。App Router 將用於路由和佈局。[cite_start] (版本：`latest`，與 `architecture.txt` 一致 [cite: 187, 188])
  - **React (19.0.0)：** 作為 Next.js 的底層 UI 函式庫，React 的元件化架構允許模組化和可重用的 UI 元素。[cite_start] (版本：`19.0.0`，與 `architecture.txt` 一致 [cite: 190, 191])
  - **TypeScript (5.7.2)：** 用於強類型、改進程式碼品質和更好的開發者體驗。[cite_start] (版本：`5.7.2`，與 `architecture.txt` 一致 [cite: 178, 179])

- **元件架構：**

  - **Shadcn UI (最新版本)：** 這個基於 Radix UI 和 Tailwind CSS 的可重用 UI 元件集合將用於基礎元素，如按鈕、卡片、對話框等。元件將透過其 CLI 添加，使其直接成為我們程式碼庫的一部分，並且易於自訂。[cite_start] (與 `architecture.txt` 一致 [cite: 198, 199])
  - **應用程式特定元件：** 將開發用於 Shadcn UI 未涵蓋的獨特 UI 部分的自訂元件 (例如，`NewsletterCard`、`PodcastPlayer`)。這些將組織在 `app/components/core/` 中。
  - **結構：** 元件預設將主要為 Server Components 以獲得最佳效能，只有在需要互動性或瀏覽器特定 API 時 (例如，事件處理器、狀態 hooks) 才會採用 Client Components (`"use client"`)。

- **狀態管理策略：**

  - **Zustand (最新版本)：** 一個輕量級、無意見且簡單的 React 狀態管理解決方案。它將用於管理需要在多個元件之間共享的全域客戶端狀態，例如 UI 狀態 (例如，podcast 播放器狀態)。[cite_start] (與 `architecture.txt` 一致 [cite: 228, 229])
  - **React Context API / Server Components：** 對於更簡單的狀態共享場景或在 Zustand 可能過度設計的情況下將資料向下傳遞到元件樹，將優先使用 React Context 或依賴 Next.js Server Component props。

- **資料流：**

  - **單向資料流：** 資料將主要從 Server Components (從 Supabase 獲取資料) 透過 props 流向 Client Components。
  - **Server Actions / Route Handlers：** 對於從 Client Components 觸發的變更或動作 (例如，未來的管理功能)，將使用 Next.js Server Actions 或專用的 API Route Handlers 與後端互動，後端進而更新 Supabase 資料庫。資料重新驗證 (例如，使用 `revalidatePath` 或 `revalidateTag`) 將用於刷新 Server Components 中的資料。
  - **Supabase Client：** Supabase JS client (用於客戶端元件的 `utils/supabase/client.ts` 和用於伺服器元件/actions 的 `utils/supabase/server.ts`) 將是與 Supabase 後端互動以進行資料獲取和變更的主要方式。

- **樣式方法：**

  - **Tailwind CSS (3.4.17)：** 一個實用程式優先的 CSS 框架，用於快速 UI 開發和一致的樣式。它將用於所有樣式，包括實現「synthwave 技術發光紫色氛圍」。[cite_start] (版本 `3.4.17`，與 `architecture.txt` 一致 [cite: 194, 195])
  - **Shadcn UI：** 利用 Tailwind CSS 實現其元件。
  - **全域樣式：** `app/globals.css` 將用於基礎 Tailwind 指令以及任何真正的全域樣式定義。
  - [cite_start] **主題自訂：** `tailwind.config.ts` 將用於擴展 Tailwind 的預設主題，添加自訂顏色 (例如，作為強調色的 synthwave 紫色，如 `#800080` [cite: 584])、字體或間距，以實現所需的美學效果。「synthwave 技術發光紫色氛圍」將透過深色基礎主題實現，互動元素、高亮和特定標題或裝飾元素的潛在微妙文字陰影或發光將使用紫色強調色。[cite_start] 字體選擇將傾向於 `ux-ui-spec.txt` 中指定的現代、簡潔的無襯線字體 [cite: 585]，如果適合主題且不影響可讀性，主要標題可能會使用更具風格的字體。

- **使用的關鍵設計模式：**
  - **Server Components 與 Client Components：** 利用 Next.js App Router 範例。
  - **Hooks：** 大量使用 React hooks (`useState`、`useEffect`、`useContext`) 和自訂 hooks 以實現可重用邏輯。
  - **Provider Pattern：** 在必要時用於 React Context API。
  - **Facade Pattern (API 互動的概念)：** Supabase 客戶端函式庫 (`utils/supabase/client.ts`、`utils/supabase/server.ts`) 作為一個 Facade，抽象化了直接的資料庫互動。資料獲取邏輯將封裝在 Server Components 或特定的資料獲取函式中。

## 詳細前端目錄結構

BMad DiCaster 前端將遵循 Next.js App Router 慣例，並基於 Vercel/Supabase Next.js App Router 範本提供的結構進行建構。主架構文件 (`docs/architecture.md`) 中定義的 monorepo 結構已經概述了頂層目錄。本節詳細說明前端特定的組織結構。

**採用的命名慣例：**

- **目錄：** `kebab-case` (例如，`app/(web)/newsletter-list/`、`app/components/core/`)
- **React 元件檔案 (.tsx)：** `PascalCase.tsx` (例如，`NewsletterCard.tsx`、`PodcastPlayer.tsx`)。Next.js App Router 特殊檔案 (例如，`page.tsx`、`layout.tsx`、`loading.tsx`、`global-error.tsx`、`not-found.tsx`) 保留其傳統的 lowercase 或 kebab-case 名稱。
- **非元件 TypeScript 檔案 (.ts)：** 主要為 `camelCase.ts` (例如，`utils.ts`、`uiSlice.ts`)。設定檔 (例如，`tailwind.config.ts`) 和共享類型定義檔案 (例如，`api-schemas.ts`、`domain-models.ts`) 可能會根據常見做法或先前的協議保留 `kebab-case`。

```plaintext
{project-root}/
├── app/                        # Next.js App Router (前端頁面、佈局、API 路由)
│   ├── (web)/                  # 使用者面向的網頁群組
│   │   ├── newsletters/        # 電子報功能路由群組
│   │   │   ├── [newsletterId]/ # 個別電子報詳細資訊的動態路由
│   │   │   │   ├── page.tsx    # 電子報詳細資訊頁面元件
│   │   │   │   └── loading.tsx # 可選：此路由的載入 UI
│   │   │   ├── page.tsx        # 電子報列表頁面元件
│   │   │   └── layout.tsx      # 可選：/newsletters 路由特定的佈局
│   │   ├── layout.tsx          # 所有 (web) 頁面的根佈局
│   │   └── page.tsx            # 首頁 (顯示電子報列表)
[cite_start] │   ├── (api)/                  # API 路由處理器 (如主架構中定義 [cite: 82, 127, 130, 133])
│   │   ├── system/
│   │   │   └── ...
│   │   └── webhooks/
│   │       └── ...
│   ├── components/             # 應用程式特定的 UI React 元件 (核心邏輯)
│   │   ├── core/               # 核心、可重用的應用程式元件
│   │   │   ├── NewsletterCard.tsx
│   │   │   ├── PodcastPlayer.tsx
│   │   │   ├── DownloadButton.tsx
│   │   │   └── BackButton.tsx
│   │   └── layout/             # 一般佈局元件
│   │       └── PageWrapper.tsx   # 頁面的一致填充/最大寬度
│   ├── auth/                   # 認證相關頁面和元件 (來自範本，MVP 前端是公開的)
│   ├── login/page.tsx          # 登入頁面 (來自範本，MVP 前端是公開的)
│   └── global-error.tsx        # 可選：自訂全域錯誤 UI (Next.js 特殊檔案)
│   └── not-found.tsx           # 可選：自訂 404 頁面 UI (Next.js 特殊檔案)
[cite_start] ├── components/                 # Shadcn UI 元件根目錄 (根據 components.json 設定 [cite: 92])
│   └── ui/                     # Shadcn 的基礎 UI 元素 (例如，Button.tsx, Card.tsx)
[cite_start] ├── lib/                        # 前端通用工具函式 [cite: 86, 309]
│   ├── utils.ts                # 通用工具函式 (日期格式化等)
│   └── hooks/                  # 自訂全域 React hooks
│       └── useScreenWidth.ts   # 範例自訂 hook
├── store/                      # Zustand 狀態管理
│   ├── index.ts                # 主 store 設定/匯出 (可以是 store.ts 或 index.ts)
│   └── slices/                 # 個別狀態 slices
│       └── podcastPlayerSlice.ts # podcast 播放器的狀態
[cite_start] ├── public/                     # 靜態資源 (圖片、favicon 等) [cite: 89]
[cite_start] │   └── logo.svg                # 應用程式標誌 (待提供 [cite: 379])
[cite_start] ├── shared/                     # 前端與 Supabase 函式之間的共享程式碼/類型 [cite: 89, 97]
│   └── types/
│       ├── api-schemas.ts      # API 請求/回應的 Zod schemas
│       └── domain-models.ts    # 核心實體類型 (HNPost, Newsletter, 等，來自主架構)
[cite_start] ├── styles/                     # 全域樣式 [cite: 90]
│   └── globals.css             # Tailwind 基礎樣式、自訂全域樣式
[cite_start] ├── utils/                      # 根工具 (來自範本 [cite: 91])
[cite_start] │   └── supabase/               # 用於前端的 Supabase 輔助函式 (來自範本 [cite: 92, 309])
│       ├── client.ts           # 客戶端 Supabase client
[cite_start] │       ├── middleware.ts       # Next.js middleware 的邏輯 (Supabase 認證 [cite: 92, 311])
│       └── server.ts           # 伺服器端 Supabase client
[cite_start] ├── tailwind.config.ts          # Tailwind CSS 設定 [cite: 93]
[cite_start] └── tsconfig.json               # TypeScript 設定 (包含路徑別名，如 '*' (參見下方檔案內容) [cite: 98, 101])
```

### 前端結構注意事項：

- **`app/(web)/`**：使用者面向頁面的路由群組。
  - [cite\_start] **`newsletters/page.tsx`**：用於列出電子報的 Server Component。[cite: 375, 573]
  - [cite\_start] **`newsletters/[newsletterId]/page.tsx`**：用於顯示單個電子報的 Server Component。[cite: 376, 576]
- **`app/components/core/`**：存放應用程式特定的 React 元件，如 `NewsletterCard.tsx`、`PodcastPlayer.tsx`、`DownloadButton.tsx`、`BackButton.tsx` (在 `ux-ui-spec.txt` 中識別)。元件遵循 `PascalCase.tsx` 命名。
- **`app/components/layout/`**：用於結構化頁面佈局的元件，例如 `PageWrapper.tsx`。元件遵循 `PascalCase.tsx` 命名。
- **`components/ui/`**：Shadcn UI 元件的標準目錄 (例如，`Button.tsx`、`Card.tsx`)。
- **`lib/hooks/`**：自訂 React hooks (例如，`useScreenWidth.ts`)，檔案遵循 `camelCase.ts` 命名。
- **`store/slices/`**：Zustand 狀態 slices。`podcastPlayerSlice.ts` 用於 podcast 播放器狀態。檔案遵循 `camelCase.ts` 命名。
- **`shared/types/`**：類型定義。`api-schemas.ts` 和 `domain-models.ts` 使用 `kebab-case.ts` 命名。
- [cite\_start] **路徑別名**：`tsconfig.json` 使用 `'*`' (參見下方檔案內容) 別名。[cite: 98, 101]

## 元件分解與實作細節

本節概述了定義 UI 元件的慣例和範本。雖然一些全域共享或基礎元件 (例如，主要佈局結構) 可能會在此處預先指定以確保一致性，但大多數功能特定元件的詳細規格將隨著 user stories 的實作而出現。關鍵是開發團隊 (或 AI 代理) 在識別出需要開發的新元件時，應遵循下方的「元件規格範本」。

---

#### 元件：`{ComponentName}` (例如，`NewsletterCard`、`PodcastPlayerControls`)

- **目的：** {簡要描述此元件的功能及其在使用者介面中的主要作用。它解決了什麼使用者需求？}
- **原始檔案：** {例如，`app/components/core/NewsletterCard.tsx`}
- **視覺參考：** {連結到特定的 Figma 框架/元件 (如果可用)，或詳細描述/草圖 (如果沒有)。如果基於 Shadcn UI 元件，請註明並說明任何關鍵自訂。}
- **Props (屬性)：**
  {列出元件接受的每個 prop。指定其名稱、TypeScript 類型、是否必需、任何預設值以及清晰的描述。}
  | Prop 名稱 | 類型 | 必需？ | 預設值 | 描述 |
  | :------------ | :------------------------------------ | :-------- | :------------ | :--------------------------------------------------- |
  | `exampleProp` | `string` | 是 | N/A | 範例字串 prop。 |
  | `items` | `Array<{id: string, name: string}>` | 是 | N/A | 要顯示的項目物件陣列。 |
  | `variant` | `'primary' \| 'secondary'` | 否 | `'primary'` | 元件的視覺變體。 |
  | `onClick` | `(event: React.MouseEvent) => void` | 否 | N/A | 可選的點擊處理器。 |
- **內部狀態 (如果有的話)：**
  {描述元件使用 React hooks (例如，`useState`) 管理的任何重要內部狀態。}
  | 狀態變數 | 類型 | 初始值 | 描述 |
  | :---------------- | :-------- | :------------ | :------------------------------------------------ |
  | `isLoading` | `boolean` | `false` | 追蹤元件的資料是否正在載入。 |
  | `selectedItem` | `string \| null` | `null` | 儲存目前選定項目的 ID。 |
- **關鍵 UI 元素 / 結構 (概念性)：**
  {描述元件的主要視覺部分及其一般佈局。如果使用 Shadcn UI 元件作為建構塊，請參考。}
  ```jsx
  // 範例 Card 元件
  <Card>
    <CardHeader>
      <CardTitle>{"{titleProp}"}</CardTitle>
      <CardDescription>{"{descriptionProp}"}</CardDescription>
    </CardHeader>
    <CardContent>{/* {項目列表或主要內容} */}</CardContent>
    <CardFooter>{/* {操作按鈕或頁腳內容} */}</CardFooter>
  </Card>
  ```
- **處理 / 發出的事件：**
  - **處理：** {列出元件直接處理的重要 DOM 事件。}
  - **發出 (回呼)：** {如果元件使用 props 向其父元件發出事件 (回呼)，請在此處列出。}
- **觸發的動作 (副作用)：**
  - **狀態管理 (Zustand)：** {如果元件與 Zustand store 互動，請指定哪個 store 和 actions。}
  - **API 呼叫 / 資料獲取：** {指定 Client Components 如何觸發變更或重新獲取 (例如，Server Actions)。}
- **樣式注意事項：**
  - {參考使用的特定 Shadcn UI 元件。}
  - {用於「synthwave」主題的關鍵 Tailwind CSS 類別或自訂樣式。}
  - {特定的響應式行為。}
- **無障礙 (AX) 注意事項：**
  - {所需的特定 ARIA 屬性。}
  - {鍵盤導航考量。}
  - {焦點管理細節。}
  - {顏色對比注意事項。}

---

_此範本將在開發過程中應用於每個新的重要元件。_

## 狀態管理深入探討

本節擴展了在「整體前端理念與模式」中概述的狀態管理策略 (Zustand)。

- [cite\_start] **選擇的解決方案：** **Zustand** (最新版本，根據 `architecture.txt` [cite: 228, 229])
- **理由：** 選擇 Zustand 是因為其簡單性、小的打包大小和無意見的特性，適合 BMad DiCaster 相對簡單的前端狀態需求 (例如，podcast 播放器狀態)。伺服器端資料主要由 Next.js Server Components 管理。

### Store 結構 / Slices

全域客戶端狀態將組織在 `store/slices/` 中的不同「slices」中。元件可以直接匯入和使用個別的 stores。

- **慣例：**
  - 每個 slice 在自己的檔案中：`store/slices/camelCaseSlice.ts`。
  - 定義狀態介面、初始狀態和動作函式。
- **核心 Slice：`podcastPlayerSlice.ts`** (用於 MVP)

  - **目的：** 管理 podcast 播放器的狀態 (當前曲目、播放狀態、時間、音量)。
  - **原始檔案：** `store/slices/podcastPlayerSlice.ts`
  - **狀態形狀 (範例)：**

    ```typescript
    interface PodcastTrack {
      id: string; // 可以是 newsletterId 或特定的音訊 ID
      title: string;
      audioUrl: string;
      duration?: number; // 以秒為單位
    }

    interface PodcastPlayerState {
      currentTrack: PodcastTrack | null;
      isPlaying: boolean;
      currentTime: number; // 以秒為單位
      volume: number; // 0 到 1
      isLoading: boolean;
      error: string | null;
    }

    interface PodcastPlayerActions {
      loadTrack: (track: PodcastTrack) => void;
      play: () => void;
      pause: () => void;
      setCurrentTime: (time: number) => void;
      setVolume: (volume: number) => void;
      setError: (message: string | null) => void;
      resetPlayer: () => void;
    }
    ```

  - **關鍵 Actions：** `loadTrack`、`play`、`pause`、`setCurrentTime`、`setVolume`、`setError`、`resetPlayer`。
  - **Zustand Store 定義：**

    ```typescript
    import { create } from "zustand";

    // 先前定義的介面：PodcastTrack, PodcastPlayerState, PodcastPlayerActions

    const initialPodcastPlayerState: PodcastPlayerState = {
      currentTrack: null,
      isPlaying: false,
      currentTime: 0,
      volume: 0.75,
      isLoading: false,
      error: null,
    };

    export const usePodcastPlayerStore = create<
      PodcastPlayerState & PodcastPlayerActions
    >((set) => ({
      ...initialPodcastPlayerState,
      loadTrack: (track) =>
        set({
          currentTrack: track,
          isLoading: true, // 假設正在載入，直到實際音訊元素確認
          error: null,
          isPlaying: false, // 載入時通常不自動播放
          currentTime: 0,
        }),
      play: () =>
        set((state) => {
          if (!state.currentTrack) return {}; // 沒有載入曲目
          return { isPlaying: true, isLoading: false, error: null };
        }),
      pause: () => set({ isPlaying: false }),
      setCurrentTime: (time) => set({ currentTime: time }),
      setVolume: (volume) => set({ volume: Math.max(0, Math.min(1, volume)) }),
      setError: (message) =>
        set({ error: message, isLoading: false, isPlaying: false }),
      resetPlayer: () => set({ ...initialPodcastPlayerState }),
    }));
    ```

### 關鍵 Selectors

Selectors 是從 store 狀態派生資料的函式。使用 Zustand，狀態通常直接從 hook 存取，但如果需要複雜的派生資料，可以使用 `reselect` 等函式庫建立 memoized selectors，儘管對於簡單情況，直接存取即可。

- **慣例：** 對於直接狀態存取，元件將使用：`const { currentTrack, isPlaying, play } = usePodcastPlayerStore();`
- **範例 Selectors (如果使用 `reselect` 或類似函式庫，用於以後更複雜的派生)：**
  - `selectCurrentTrackTitle`：返回 `state.currentTrack?.title || 'No track loaded'`。
  - `selectIsPodcastPlaying`：返回 `state.isPlaying`。

### 關鍵 Actions / Reducers / Thunks

Zustand actions 是在 `create` 呼叫中定義的函式，它們使用 `set` 更新狀態。非同步操作 (例如獲取資料，儘管對於通常用於 UI 狀態的 Zustand 來說較不常見) 可以透過在這些 actions 中呼叫非同步函式，然後在完成時呼叫 `set` 來處理。

- **慣例：** Actions 是 store hook 的一部分：`const { loadTrack } = usePodcastPlayerStore();`。
- **非同步範例 (概念性，如果一個 slice 需要獲取資料)：**
  ```typescript
  // 在假設的 userSettingsSlice.ts 中
  // fetchUserSettings: async () => {
  //   set({ isLoading: true });
  //   try {
  //     const settings = await api.fetchUserSettings(); // api 是匯入的 service
  //     set({ userSettings: settings, isLoading: false });
  //   } catch (error) {
  //     set({ error: 'Failed to fetch settings', isLoading: false });
  //   }
  // }
  ```
  對於 BMad DiCaster MVP，大多數資料獲取是透過 Server Components 進行的。Zustand 中的客戶端非同步 actions 主要用於與伺服器資料獲取不直接相關的客戶端特定操作。

## API 互動層

前端將與 Supabase 互動以獲取資料。Server Components 將使用伺服器端 Supabase client 直接獲取資料。需要變更資料或觸發後端邏輯的 Client Components 將使用 Next.js Server Actions，或者在必要時使用專用的 Next.js API Route Handlers，然後這些處理器再與 Supabase 互動。

### Client/Service 結構

- **HTTP Client 設定 (如果廣泛使用 Next.js API Route Handlers)：**

  - 雖然 Server Components 和 Server Actions 是 Supabase 互動的首選，但如果需要從客戶端直接呼叫自訂 Next.js API 路由，可以使用簡單的 `Workspace` 包裝器或輕量級 client，如 `ky`。
  - Vercel/Supabase 範本提供了 `utils/supabase/client.ts` (用於客戶端元件) 和 `utils/supabase/server.ts` (用於 Server Components、Route Handlers、Server Actions)。這些將是與 Supabase 互動的主要介面。
  - **Base URL：** 不適用於直接 Supabase client 使用。對於自訂 API 路由：相對路徑 (例如，`/api/my-route`)。
  - **認證：** Supabase clients 處理認證 token 管理。[cite\_start] 對於自訂 API 路由，Next.js middleware (`middleware.ts` [cite: 92, 311]) 將處理 session 驗證。

- **Service 定義 (Supabase 資料存取的概念性)：**

  - 對於使用 Server Components 獲取資料，嚴格來說不需要單獨的「service」檔案，如 `userService.ts`。資料獲取邏輯將與 Server Components 或 Server Actions 共同存放。
  - **範例 (在 Server Component 中獲取資料)：**

    ```typescript
    // app/(web)/newsletters/page.tsx
    import { createClient } from "'utils/supabase/server"' (see below for file content);
    import NewsletterCard from "'app/components/core/NewsletterCard"' (see below for file content); // Corrected path

    export default async function NewsletterListPage() {
      const supabase = createClient();
      const { data: newsletters, error } = await supabase
        .from("newsletters")
        .select("id, title, target_date, podcast_url") // Add podcast_url
        .order("target_date", { ascending: false });

      if (error) console.error("Error fetching newsletters:", error);
      // Render newsletters or error state
    }
    ```

  - **範例 (用於假設的「訂閱」功能的 Server Action - 未來範圍)：**

    ```typescript
    // app/actions/subscribeActions.ts
    "use server";
    import { createClient } from "'utils/supabase/server"' (see below for file content);
    import { z } from "zod";
    import { revalidatePath } from "next/cache";

    const EmailSchema = z.string().email();

    export async function subscribeToNewsletter(email: string) {
      const validation = EmailSchema.safeParse(email);
      if (!validation.success) {
        return { error: "Invalid email format." };
      }
      const supabase = createClient();
      const { error } = await supabase
        .from("subscribers")
        .insert({ email: validation.data });
      if (error) {
        return { error: "Subscription failed." };
      }
      revalidatePath("/"); // 範例路徑重新驗證
      return { success: true };
    }
    ```

### 錯誤處理與重試 (前端)

- **Server Component 資料獲取錯誤：** Server Components 中來自 Supabase 的錯誤應被捕獲。元件可以隨後渲染適當的錯誤 UI 或將錯誤資訊作為 props 傳遞。Next.js 錯誤處理 (例如 `error.tsx` 檔案) 也可用於不可恢復的錯誤。
- **Client Component / Server Action 錯誤：**
  - Server Actions 應返回結構化的回應 (例如，`{ success: boolean, data?: any, error?: string }`)。呼叫 Server Actions 的 Client Components 將處理這些回應以更新 UI (例如，顯示錯誤訊息、toast 通知)。
  - Shadcn UI 包含一個 `Toast` 元件，可用於非模態錯誤通知。
- **UI 錯誤邊界：** React Error Boundaries 可以在元件樹的關鍵點 (例如，主要佈局部分或複雜元件周圍) 實作，以捕獲 Client Components 中的渲染錯誤並顯示備用 UI，防止應用程式完全崩潰。根目錄的 `global-error.tsx` 可以作為全域邊界。
- **重試邏輯：** 通常，資料獲取的重試邏輯應由使用者處理 (例如，「重試」按鈕)，而不是 MVP 的自動客戶端重試，除非處理特定的、已知的暫時性問題。Supabase client 函式庫可能對某些類型的網路錯誤有自己的內部重試機制。

## 路由策略

導航和路由將由 Next.js App Router 處理。

- [cite\_start] **路由函式庫：** **Next.js App Router** (根據 `architecture.txt` [cite: 187, 188, 308])

### 路由定義

[cite\_start] 基於 `ux-ui-spec.txt` 和 PRD [cite: 352, 375, 376]。

| 路徑模式                      | 元件/頁面 (`app/(web)/...`)           | 保護 | 注意事項                                               |
| :---------------------------- | :------------------------------------ | :--- | :----------------------------------------------------- |
| `/`                           | `newsletters/page.tsx` (實際上)       | 公開 | 首頁顯示電子報列表。                                   |
| `/newsletters`                | `newsletters/page.tsx`                | 公開 | 顯示當前和過去電子報的列表。                           |
| `/newsletters/[newsletterId]` | `newsletters/[newsletterId]/page.tsx` | 公開 | 顯示選定電子報的詳細資訊頁面。`newsletterId` 是 UUID。 |

[cite\_start] _(注意：主架構文件 [cite: 83] 顯示首頁使用 `app/page.tsx`。對於 MVP，這可以重定向到 `/newsletters` 或直接渲染電子報列表內容。上表假設它實際上提供電子報列表。)_

### 路由守衛 / 保護

- **認證守衛：** MVP 前端是公開的，無需使用者登入即可顯示電子報和 podcast。[cite\_start] Vercel/Supabase 範本包含 middleware (`middleware.ts` [cite: 92, 311])，用於根據 Supabase Auth 保護路由。這對於未來的管理部分將是相關的，但在 MVP 中不主動用於限制一般使用者的內容。
- **授權守衛：** 不適用於 MVP。

## 建構、打包與部署

詳細資訊與 Vercel 平台和 Next.js 功能一致。

### 建構流程與腳本

- **關鍵建構腳本：**
  - `npm run dev`：啟動 Next.js 本地開發伺服器。
  - `npm run build`：生成 Next.js 應用程式的優化生產建構。(來自 `package.json` 的腳本)
  - `npm run start`：在建構後啟動 Next.js 生產伺服器。
- **建構期間的環境變數處理：**
  - 客戶端變數必須以 `NEXT_PUBLIC_` 為前綴 (例如，`NEXT_PUBLIC_SUPABASE_URL`、`NEXT_PUBLIC_SUPABASE_ANON_KEY`)。
  - 伺服器端變數 (用於 Server Components、Server Actions、Route Handlers) 直接透過 `process.env` 存取。
  - 環境變數在 Vercel 專案設定中針對不同環境 (生產、預覽、開發) 進行管理。本地開發使用 `.env.local`。

### 關鍵打包優化

- **程式碼分割：** Next.js App Router 自動執行基於路由的程式碼分割。如果需要進一步的元件級程式碼分割，可以使用動態匯入 (`next/dynamic`)。
- **Tree Shaking：** 在生產建構過程中由 Next.js 的 Webpack 設定確保。
- **延遲載入：** Next.js 預設處理路由段的延遲載入。圖片 (`next/image`) 經過優化，可以延遲載入。
- **最小化與壓縮：** 在 `npm run build` 期間由 Next.js 自動處理 (JavaScript、CSS 最小化；Gzip/Brotli 壓縮通常由 Vercel 處理)。

### 部署到 CDN/Hosting

- [cite\_start] **目標平台：** **Vercel** (根據 `architecture.txt` [cite: 206, 207, 382])
- **部署觸發：** 透過 Vercel 的 Git 整合 (GitHub) 在推送到指定分支 (例如，`main` 用於生產，PR 分支用於預覽) 時自動部署。[cite\_start] (與 `architecture.txt` 一致 [cite: 279, 280])
- **資源快取策略：** Vercel 的 Edge Network 處理靜態資源和 Server Component payloads 的 CDN 快取。快取控制標頭將根據 Next.js 預設值進行設定，並可在必要時自訂 (例如，對於 `public/` 資源)。

## 前端測試策略

本節詳細闡述了在 `architecture.txt` 中定義的整體測試策略，重點關注前端特定內容。

- **主測試策略連結：** `docs/architecture.md#overall-testing-strategy` (以及 `docs/architecture.md#coding-standards` 用於測試檔案的共同存放)。

### 元件測試

- **範圍：** 獨立測試個別 React 元件，主要關注基於 props 的 UI 渲染和基本互動。
- [cite\_start] **工具：** **Jest** (測試執行器、斷言函式庫、mocking，根據 `architecture.txt` [cite: 236, 237][cite\_start]) 和 **React Testing Library (RTL)** (用於以使用者為中心的元件查詢和互動，根據 `architecture.txt` [cite: 232, 233])。
- **重點：**
  - 基於 props 的正確渲染。
  - 使用者互動 (例如，按鈕點擊觸發回呼)。
  - 條件渲染邏輯。
  - 無障礙屬性。
- [cite\_start] **位置：** 測試檔案 (`*.test.tsx` 或 `*.spec.tsx`) 將與元件檔案共同存放 (例如，`app/components/core/NewsletterCard.test.tsx`)。[cite: 99, 295]
- **範例準則：** 「`NewsletterCard` 元件應渲染作為 props 傳遞的標題和日期。點擊卡片應導航 (mocked) 或呼叫 `onClick` prop。」

### UI 整合/流程測試

- **範圍：** 測試組成 UI 或小型使用者流程的多個元件之間的互動，可能使用 mocked Supabase client 回應或 Zustand store 狀態。
- **工具：** Jest 和 React Testing Library。
- **重點：**
  - 父元件與其子元件之間的資料流。
  - Zustand store 中的狀態更新影響多個元件。
  - 在簡單流程中渲染一系列 UI 元素 (例如，從列表中選擇一個項目並查看詳細資訊出現)。
- **範例準則：** 「`NewsletterListPage` 在提供 mock 電子報資料時，應正確渲染多個 `NewsletterCard` 元件。點擊卡片應正確調用導航邏輯。」

### 端到端 UI 測試工具與範圍

- [cite\_start] **工具：** **Playwright** (根據 `architecture.txt` [cite: 240, 241])。
- **範圍 (前端重點)：**
  - 驗證「查看電子報」使用者流程：
    1.  導航到電子報列表頁面。
    2.  驗證電子報是否已列出。
    3.  點擊電子報。
    4.  驗證電子報詳細資訊頁面載入並包含內容。
    5.  驗證如果存在 podcast URL，podcast 播放器是否存在。
    6.  驗證下載按鈕是否存在。
    7.  驗證「返回列表」按鈕是否有效。
  - 關鍵頁面 (列表和詳細資訊) 的基本行動裝置響應性檢查。
- **UI 測試資料管理：** E2E 測試將依賴於開發 Supabase 實例中填充的資料，或者如果使用 Playwright 的網路攔截針對獨立的前端測試，則使用 mocked API 回應。對於針對實時開發環境的真正 E2E 測試，將使用 Supabase 開發實例中預先填充的資料。

## 無障礙 (AX) 實作細節

[cite\_start] 前端將遵循 **WCAG 2.1 Level A** 作為最低目標，如 `docs/ui-ux-spec.txt` 中指定 [cite: 588]。

- [cite\_start] **語義化 HTML：** 強調使用正確的 HTML5 元素 (`<nav>`、`<main>`、`<article>`、`<aside>`、`<button>`) 以提供固有的含義和結構。[cite: 589]
- **ARIA 實作：**
  - Shadcn UI 元件在建構時考慮了無障礙性，通常包含適當的 ARIA 屬性。
  - 對於自訂元件，將使用相關的 ARIA 角色 (例如，`role="region"`、`role="alert"`) 和屬性 (例如，`aria-label`、`aria-describedby`、`aria-live`、`aria-expanded`)，用於動態內容、互動元素和自訂小部件，以確保輔助技術能夠正確解釋它們。
- [cite\_start] **鍵盤導航：** 所有互動元素 (連結、按鈕、輸入框、自訂控制項) 必須可聚焦，並且只能使用鍵盤按邏輯順序操作。[cite: 588] 焦點指示器將清晰可見。
- **焦點管理：** 對於動態 UI 元素，如模態框或非原生下拉選單 (如果建構了超出 Shadcn 功能的自訂元素)，將以程式方式管理焦點，以確保它在適當時移動到元素內部並被困在其中，並在關閉時返回到觸發元素。
- **替代文字：** 所有有意義的圖片都將具有描述性的 `alt` 文字。[cite\_start] 裝飾性圖片將具有空的 `alt=""`。[cite: 590]
- **顏色對比：** 遵守 WCAG 2.1 Level A 的文字和互動元素與其背景之間的顏色對比度。[cite\_start] 將仔細選擇「synthwave」主題的紫色強調色 [cite: 584] 以滿足這些要求。[cite\_start] 將使用工具驗證對比度。[cite: 591]
- **AX 測試工具：**
  - 自動化：Axe DevTools 瀏覽器擴充功能、Lighthouse 無障礙性審核。
  - 手動：僅鍵盤導航測試、螢幕閱讀器測試 (例如，NVDA、VoiceOver) 關鍵使用者流程。

## 效能考量

[cite\_start] 目標是快速載入且響應迅速的使用者體驗。[cite: 360, 565]

- **圖片優化：**
  - 使用 `next/image` 進行自動圖片優化 (調整大小、支援 WebP 格式、預設延遲載入)。
- **程式碼分割與延遲載入：**
  - Next.js App Router 處理基於路由的程式碼分割。
  - `next/dynamic` 用於客戶端延遲載入不立即可見或較重的元件。
- **最小化重新渲染 (React)：**
  - 對於使用相同 props 頻繁渲染的元件，謹慎使用 `React.memo`。
  - 如果引入複雜的派生狀態，優化 Zustand selectors (儘管直接存取通常足夠)。
  - 盡可能確保穩定的 prop 引用。
- **防抖/節流：** MVP 功能預計不會用到，但將考慮用於未來的互動元素，如搜尋輸入框。
- **虛擬化：** 考慮到項目數量有限 (例如，每天 30 篇電子報)，MVP 預計不會用到虛擬化。如果未來列表變得非常長，將考慮使用 TanStack Virtual 等虛擬化函式庫。
- **快取策略 (客戶端)：**
  - 利用 Next.js 內建的快取功能，透過 Vercel 的 Edge Network 快取 Server Component payloads 和靜態資源。
  - 靜態資源 (`public/` 資料夾) 的瀏覽器快取將使用 Vercel 設定的預設最佳標頭。
- **效能監控工具：**
  - 瀏覽器開發者工具 (Performance tab、Lighthouse)。
  - Vercel Analytics (如果啟用) 用於真實使用者監控。
  - WebPageTest 用於詳細的效能分析。
- **打包大小分析：** 如果打包大小成為問題，使用 `@next/bundle-analyzer` 等工具檢查生產打包，並找出優化機會。

## 變更日誌

| 日期       | 版本 | 描述                     | 作者               |
| :--------- | :--- | :----------------------- | :----------------- |
| 2025-05-13 | 0.1  | 前端架構文件的初始草稿。 | 4-design-arch (AI) |
