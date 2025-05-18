# 我的前端技術偏好（Next.js & React）

僅供自己與 AI 參考，說明我偏好如何構建 Next.js 應用。目標是保持風格一致。

## 整體專案結構

- **`/app` 目錄用於路由（Next.js App Router）：**
  - 很喜歡這個新路由。將 UI 與邏輯依路由分組很合理。
  - `layout.tsx` 與 `page.tsx` 作為每個路由的主要檔案。
  - 載入畫面 UI：`loading.tsx` 很不錯。
  - 錯誤畫面 UI：`error.tsx` 用於攔截錯誤。
- **`/components` 目錄（根目錄）：**
  - 放所有共用／可重用的 React 元件。
  - 每個元件一個子資料夾，例如 `/components/Button/Button.tsx`、`/components/Button/Button.stories.tsx`、`/components/Button/Button.test.tsx`。
  - 我喜歡將 stories 與單元測試與元件放在一起。
- **`/lib` 目錄（根目錄）：**
  - 放協助函式、工具、常數、型別定義等非元件內容。
  - 例如 `lib/utils.ts`、`lib/hooks.ts`、`lib/types.ts`
- **`/styles` 目錄（根目錄）：**
  - 全域樣式放在 `globals.css`。
  - 若有使用 Tailwind CSS，這裡也會放設定檔（如 `tailwind.config.js`）。
- **`/public` 目錄：** 用於靜態資源，與一般慣例相同。
- **`/store` 或 `/contexts` 目錄（根目錄）：**
  - 用於狀態管理。若用 Zustand，可能是 `/store/userStore.ts`；若用 React Context，則是 `/contexts/ThemeContext.tsx`。

## React & Next.js 慣例

- **全面使用 TypeScript：**
  - 一定要。能幫助發現許多錯誤。
  - 公開 API 型別定義（props）用 `interface`，內部／工具型別用 `type`。
- **使用 Hooks 的函式型元件：**
  - 標準做法。
- **Server Components 與 Client Components（Next.js App Router）：**
  - 預設盡量使用 Server Components 以提升效能。
  - 僅在必要時（事件處理、狀態、僅限瀏覽器 API）加 `"use client";`。
- **路由設計：**
  - 採用 Next.js App Router。
  - 動態路由用 `[]` 與 `[... ]` 資料夾。
- **API 路由：**
  - 放在 `/app/api/...` 目錄下。
  - 適合小型 backend-for-frontend 任務。
- **狀態管理：**
  - **Zustand：** 我主要用於全域狀態管理。簡單，樣板碼比 Redux 少。
  - **React Context：** 適合較簡單、區域性的狀態共享（如主題、用戶驗證狀態不複雜時）。
  - 避免 Prop Drilling：若 props 超過 2-3 層且未被使用，建議用 Zustand 或 Context。
- **元件設計：**
  - 小而專注的元件。
  - props 應明確，並解構 props。
  - 元件型別建議用 `React.FC<Props>`。
  - 無需額外 DOM 節點時使用 Fragment（`<>...</>`）。
- **樣式設計：**
  - **Tailwind CSS：** 強烈偏好。能快速設計樣式，且比大量 CSS Modules 檔案更乾淨。
  - 若 Tailwind 不適用，則以 CSS Modules 作為備用或針對特定元件樣式。
- **環境變數：**
  - 客戶端可存取的環境變數加上 `NEXT_PUBLIC_` 前綴。
  - 本地開發用標準 `.env.local`。

## 測試

- **E2E 測試工具：Playwright**
  - 很喜歡 Playwright。速度快且功能強大（自動等待、選擇器好用）。
  - 測試放在根目錄的 `/e2e` 資料夾。
  - Page Object Model（POM）模式有助於維護 E2E 測試。
    - 例如 `e2e/pages/loginPage.ts`、`e2e/tests/auth.spec.ts`
- **單元測試：Jest & React Testing Library**
  - React 元件的標準測試工具。
  - 著重於從使用者角度測試元件行為，而非實作細節。
  - 使用 `jest.mock` 模擬 API 呼叫。
- **整合測試：**
  - 可以用 Jest/RTL 測多個元件協作，或用 Playwright 測小型使用者流程。

## 其他偏好函式庫／工具

- **程式碼檢查／格式化：**
  - ESLint 採用標準設定（如 `eslint-config-next`）。
  - Prettier 用於程式碼格式化。
  - 於 pre-commit hook 執行（Husky + lint-staged）。
- **資料擷取：**
  - Next.js 內建 `fetch`（支援 server components、自動快取）。
  - 若客戶端資料擷取需求複雜（快取、重新驗證等），可用 SWR 或 React Query，但優先考慮用 Server Components 擷取資料。
- **表單處理：**
  - React Hook Form：適合處理含驗證的表單。
  - Zod 進行 schema 驗證。
- **Storybook：**
  - 會使用於元件開發與 UI 文件。
  - stories 檔案與元件放一起。

## 我仍在學習／探索的內容：

- 進階 Next.js 功能（更深入的 Route Handlers、Server Actions）。
- 更進階的 server components 測試策略。
- 超越基礎的效能優化。
