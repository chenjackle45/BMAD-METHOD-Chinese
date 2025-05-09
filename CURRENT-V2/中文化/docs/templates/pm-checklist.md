# 產品經理 (PM) 需求檢查清單

此檢查清單作為一個全面的框架，用於確保產品需求文件 (PRD) 和史詩定義完整、結構良好且適當地範疇化以進行 MVP 開發。PM 應在產品定義過程中系統地檢查每個項目。

## 1. 問題定義與背景

### 1.1 問題陳述

- [ ] 清楚表述所解決的問題
- [ ] 確定誰遇到了這個問題
- [ ] 解釋為什麼解決這個問題很重要
- [ ] 量化問題的影響 (如果可能)
- [ ] 與現有解決方案的區別

### 1.2 業務目標與成功指標

- [ ] 定義具體、可衡量的業務目標
- [ ] 建立清晰的成功指標和 KPI
- [ ] 指標與使用者和業務價值相關聯
- [ ] 確定基線測量 (如果適用)
- [ ] 指定實現目標的時間框架

### 1.3 使用者研究與洞察

- [ ] 明確定義目標使用者角色
- [ ] 記錄使用者需求和痛點
- [ ] 總結使用者研究結果 (如果有)
- [ ] 包括競爭分析
- [ ] 提供市場背景

## 2. MVP 範疇定義

### 2.1 核心功能

- [ ] 明確區分必要功能與非必要功能
- [ ] 功能直接解決定義的問題陳述
- [ ] 每個功能與具體使用者需求相關聯
- [ ] 從使用者角度描述功能
- [ ] 定義成功的最低要求

### 2.2 範疇邊界

- [ ] 清楚表述範疇外的內容
- [ ] 包括未來增強部分
- [ ] 記錄範疇決策的理由
- [ ] MVP 在最小化功能的同時最大化學習
- [ ] 範疇已多次審查和完善

### 2.3 MVP 驗證方法

- [ ] 定義測試 MVP 成功的方法
- [ ] 計劃初步使用者反饋機制
- [ ] 指定超越 MVP 的標準
- [ ] 闡明 MVP 的學習目標
- [ ] 設定時間表期望

## 3. 使用者體驗需求

### 3.1 使用者旅程與流程

- [ ] 記錄主要使用者流程
- [ ] 確定每個流程的進入點和退出點
- [ ] 繪製決策點和分支
- [ ] 突出關鍵路徑
- [ ] 考慮邊界情況

### 3.2 可用性需求

- [ ] 記錄無障礙考量
- [ ] 指定平台/設備相容性
- [ ] 從使用者角度定義效能期望
- [ ] 概述錯誤處理和恢復方法
- [ ] 確定使用者反饋機制

### 3.3 UI 需求

- [ ] 概述資訊架構
- [ ] 確定關鍵 UI 元件
- [ ] 參考視覺設計指南 (如果適用)
- [ ] 指定內容需求
- [ ] 定義高層次導航結構

## 4. 功能需求

### 4.1 功能完整性

- [ ] 記錄 MVP 所需的所有功能
- [ ] 功能具有清晰、以使用者為中心的描述
- [ ] 指明功能的優先級/重要性
- [ ] 需求是可測試和可驗證的
- [ ] 確定功能之間的依賴關係

### 4.2 需求品質

- [ ] 需求具體且明確
- [ ] 需求專注於「做什麼」而非「如何做」
- [ ] 需求使用一致的術語
- [ ] 將複雜需求分解為更簡單的部分
- [ ] 最小化或解釋技術術語

### 4.3 使用者故事與驗收標準

- [ ] 故事遵循一致的格式
- [ ] 驗收標準是可測試的
- [ ] 故事大小適當 (不過大)
- [ ] 故事在可能的情況下是獨立的
- [ ] 故事包含必要的背景

## 5. 非功能需求

### 5.1 效能需求

- [ ] 定義響應時間期望
- [ ] 指定吞吐量/容量需求
- [ ] 記錄延展性需求
- [ ] 確定資源使用限制
- [ ] 設定負載處理期望

### 5.2 安全性與合規性

- [ ] 指定數據保護需求
- [ ] 定義身份驗證/授權需求
- [ ] 記錄合規需求
- [ ] 概述安全測試需求
- [ ] 解決隱私考量

### 5.3 可靠性與韌性

- [ ] 定義可用性需求
- [ ] 記錄備份和恢復需求
- [ ] 設定容錯期望
- [ ] 指定錯誤處理需求
- [ ] 包括維護和支持考量

### 5.4 技術限制

- [ ] 記錄平台/技術限制
- [ ] 概述整合需求
- [ ] 確定第三方服務依賴
- [ ] 指定基礎設施需求
- [ ] 確定開發環境需求

## 6. 史詩與故事結構

### 6.1 史詩定義

- [ ] 史詩代表連貫的功能單元
- [ ] 史詩專注於使用者/業務價值交付
- [ ] 清楚闡明史詩目標
- [ ] 史詩大小適當以便增量交付
- [ ] 確定史詩的順序和依賴關係

### 6.2 故事分解

- [ ] 故事分解為適當大小
- [ ] 故事具有清晰、獨立的價值
- [ ] 故事包含適當的驗收標準
- [ ] 記錄故事的依賴關係和順序
- [ ] 故事與史詩目標一致

### 6.3 第一個史詩的完整性

- [ ] 第一個史詩包括所有必要的設置步驟
- [ ] 涵蓋專案腳手架和初始化
- [ ] 包括核心基礎設施設置
- [ ] 涵蓋開發環境設置
- [ ] 早期建立本地可測試性

## 7. 技術指導

### 7.1 架構指導

- [ ] 提供初步架構方向
- [ ] 清楚傳達技術限制
- [ ] 確定整合點
- [ ] 突出效能考量
- [ ] 闡明安全需求

### 7.2 技術決策框架

- [ ] 提供技術選擇的決策標準
- [ ] 闡明關鍵決策的權衡
- [ ] 突出不可協商的技術需求
- [ ] 確定需要技術調查的領域
- [ ] 提供技術債務處理指導

### 7.3 實施考量

- [ ] 提供開發方法指導
- [ ] 闡明測試需求
- [ ] 設定部署期望
- [ ] 確定監控需求
- [ ] 指定文件需求

## 8. 跨功能需求

### 8.1 數據需求

- [ ] 確定數據實體和關係
- [ ] 指定數據存儲需求
- [ ] 定義數據品質需求
- [ ] 確定數據保留政策
- [ ] 解決數據遷移需求 (如果適用)

### 8.2 整合需求

- [ ] 確定外部系統整合
- [ ] 記錄 API 需求
- [ ] 指定整合身份驗證需求
- [ ] 定義數據交換格式
- [ ] 概述整合測試需求

### 8.3 運營需求

- [ ] 設定部署頻率期望
- [ ] 定義環境需求
- [ ] 確定監控和警報需求
- [ ] 記錄支持需求
- [ ] 指定效能監控方法

## 9. 清晰度與溝通

### 9.1 文件品質

- [ ] 文件使用清晰、一致的語言
- [ ] 文件結構良好且組織有序
- [ ] 必要時定義技術術語
- [ ] 包含有助於理解的圖表/視覺化
- [ ] 文件適當版本化

### 9.2 利益相關者對齊

- [ ] 確定關鍵利益相關者
- [ ] 納入利益相關者的意見
- [ ] 解決潛在分歧領域
- [ ] 建立更新的溝通計劃
- [ ] 定義批准流程

## PRD 與史詩驗證摘要

### 類別狀態

| 類別              | 狀態               | 關鍵問題 |
| ----------------- | ------------------ | -------- |
| 1. 問題定義與背景 | 通過/失敗/部分通過 |          |
| 2. MVP 範疇定義   | 通過/失敗/部分通過 |          |
| 3. 使用者體驗需求 | 通過/失敗/部分通過 |          |
| 4. 功能需求       | 通過/失敗/部分通過 |          |
| 5. 非功能需求     | 通過/失敗/部分通過 |          |
| 6. 史詩與故事結構 | 通過/失敗/部分通過 |          |
| 7. 技術指導       | 通過/失敗/部分通過 |          |
| 8. 跨功能需求     | 通過/失敗/部分通過 |          |
| 9. 清晰度與溝通   | 通過/失敗/部分通過 |          |

### 關鍵缺陷

- 列出所有在交付給架構師之前必須解決的關鍵問題

### 建議

- 提供具體建議以解決每個缺陷

### 最終決定

- **準備交付架構師**: PRD 和史詩是全面的、結構良好的，並準備好進行架構設計。
- **需要改進**: 需求文件需要額外工作以解決已識別的缺陷。
