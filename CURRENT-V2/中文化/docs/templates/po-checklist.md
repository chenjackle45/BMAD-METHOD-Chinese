# 產品負責人 (PO) 驗證清單

此清單作為產品負責人在開發執行前驗證完整 MVP 計畫的全面框架。PO 應系統性地檢查每個項目，記錄符合狀態並註明任何缺失。

## 1. 專案設定與初始化

### 1.1 專案腳手架

- [ ] Epic 1 包含專案建立/初始化的明確步驟
- [ ] 如果使用起始範本，包含複製/設定的步驟
- [ ] 如果從頭開始建構，定義所有必要的腳手架步驟
- [ ] 包含初始 README 或文件設定
- [ ] 定義版本庫設定與初始提交流程（如果適用）

### 1.2 開發環境

- [ ] 明確定義本地開發環境設定
- [ ] 指定所需工具與版本（Node.js、Python 等）
- [ ] 包含安裝相依套件的步驟
- [ ] 涉及配置檔案（dotenv、配置檔案等）
- [ ] 包含開發伺服器設定

### 1.3 核心相依性

- [ ] 在過程早期安裝所有關鍵套件/函式庫
- [ ] 妥善處理套件管理（npm、pip 等）
- [ ] 適當定義版本規範
- [ ] 註明相依性衝突或特殊需求

## 2. 基礎設施與部署排序

### 2.1 資料庫與資料存儲設定

- [ ] 在任何資料庫操作之前選擇/設定資料庫
- [ ] 在資料操作之前建立 Schema 定義
- [ ] 如果適用，定義遷移策略
- [ ] 如果需要，包含種子資料或初始資料設定
- [ ] 早期建立資料庫存取模式與安全性

### 2.2 API 與服務配置

- [ ] 在實作端點之前設定 API 框架
- [ ] 在實作服務之前建立服務架構
- [ ] 在受保護路由之前設定驗證框架
- [ ] 在使用之前建立中介軟體與通用工具

### 2.3 部署管線

- [ ] 在任何部署操作之前建立 CI/CD 管線
- [ ] 在使用之前設定基礎設施即程式碼 (IaC)
- [ ] 早期定義環境配置（開發、預備、正式）
- [ ] 在實作之前定義部署策略
- [ ] 涉及回滾程序或考量

### 2.4 測試基礎設施

- [ ] 在撰寫測試之前安裝測試框架
- [ ] 測試環境設定先於測試實作
- [ ] 在測試之前定義模擬服務或資料
- [ ] 在使用之前建立測試工具或輔助工具

## 3. 外部相依性與整合

### 3.1 第三方服務

- [ ] 為所需服務識別帳戶建立步驟
- [ ] 定義 API 金鑰獲取流程
- [ ] 包含安全儲存憑證的步驟
- [ ] 考慮回退或離線開發選項

### 3.2 外部 API

- [ ] 明確識別與外部 API 的整合點
- [ ] 正確排序與外部服務的驗證
- [ ] 承認 API 限制或約束
- [ ] 考慮 API 故障的備援策略

### 3.3 基礎設施服務

- [ ] 正確排序雲端資源配置
- [ ] 識別 DNS 或網域註冊需求
- [ ] 如果需要，包含電子郵件或訊息服務設定
- [ ] 靜態資產託管設定先於使用

## 4. 使用者/代理責任劃分

### 4.1 使用者行動

- [ ] 使用者責任僅限於需要人工介入的部分
- [ ] 正確分配外部服務的帳戶建立給使用者
- [ ] 正確分配購買或付款行動給使用者
- [ ] 適當分配憑證提供給使用者

### 4.2 開發代理行動

- [ ] 所有與程式碼相關的任務分配給開發代理
- [ ] 正確識別自動化流程為代理責任
- [ ] 正確分配配置管理
- [ ] 測試與驗證分配給適當代理

## 5. 功能排序與相依性

### 5.1 功能相依性

- [ ] 依賴其他功能的功能排序正確
- [ ] 在使用之前建立共用元件
- [ ] 使用者流程遵循邏輯進程
- [ ] 驗證功能先於受保護路由/功能

### 5.2 技術相依性

- [ ] 在高階服務之前建立低階服務
- [ ] 在使用之前建立函式庫與工具
- [ ] 在操作之前定義資料模型
- [ ] 在客戶端使用之前定義 API 端點

### 5.3 跨 Epic 相依性

- [ ] 後續 Epic 建立在早期 Epic 的功能上
- [ ] 沒有 Epic 需要來自後續 Epic 的功能
- [ ] 早期 Epic 建立的基礎設施一致使用
- [ ] 維持增量價值交付

## 6. MVP 範疇對齊

### 6.1 PRD 目標對齊

- [ ] Epic/故事中涵蓋 PRD 定義的所有核心目標
- [ ] 功能直接支持定義的 MVP 目標
- [ ] 不包含超出 MVP 範疇的額外功能
- [ ] 適當優先考量關鍵功能

### 6.2 使用者旅程完整性

- [ ] 完全實作所有關鍵使用者旅程
- [ ] 涉及邊界情況與錯誤場景
- [ ] 包含使用者體驗考量
- [ ] 如果有指定，納入無障礙需求

### 6.3 技術需求滿足

- [ ] 涵蓋 PRD 中的所有技術限制
- [ ] 納入非功能性需求
- [ ] 架構決策符合指定限制
- [ ] 適當考量效能需求

## 7. 風險管理與實際性

### 7.1 技術風險緩解

- [ ] 對於複雜或不熟悉的技術有適當的學習/原型故事
- [ ] 高風險元件有明確的驗證步驟
- [ ] 對於高風險整合有備援策略
- [ ] 對效能問題有明確的測試/驗證

### 7.2 外部相依性風險

- [ ] 承認並緩解與第三方服務的風險
- [ ] 涉及 API 限制或約束
- [ ] 對於關鍵外部服務有備援策略
- [ ] 考慮外部服務的成本影響

### 7.3 時間表實際性

- [ ] 故事複雜性與排序顯示合理的時間表
- [ ] 將對外部因素的依賴最小化或管理
- [ ] 在可能的情況下啟用平行工作
- [ ] 識別並優化關鍵路徑

## 8. 文件與交接

### 8.1 開發者文件

- [ ] 與實作同步建立 API 文件
- [ ] 設定指導完整
- [ ] 記錄架構決策
- [ ] 記錄模式與慣例

### 8.2 使用者文件

- [ ] 如果需要，包含使用者指南或幫助文件
- [ ] 考慮錯誤訊息與使用者回饋
- [ ] 完全指定上手流程
- [ ] 如果適用，定義支持流程

## 9. MVP 後考量

### 9.1 未來增強

- [ ] 明確區分 MVP 與未來功能
- [ ] 架構支持計畫中的未來增強
- [ ] 記錄技術債務考量
- [ ] 識別擴展性點

### 9.2 回饋機制

- [ ] 如果需要，納入分析或使用追蹤
- [ ] 考慮使用者回饋收集
- [ ] 涉及監控與警報
- [ ] 納入效能測量

## 驗證摘要

### 類別狀態

| 類別                   | 狀態           | 關鍵問題 |
| ---------------------- | -------------- | -------- |
| 1. 專案設定與初始化    | 通過/失敗/部分 |          |
| 2. 基礎設施與部署排序  | 通過/失敗/部分 |          |
| 3. 外部相依性與整合    | 通過/失敗/部分 |          |
| 4. 使用者/代理責任劃分 | 通過/失敗/部分 |          |
| 5. 功能排序與相依性    | 通過/失敗/部分 |          |
| 6. MVP 範疇對齊        | 通過/失敗/部分 |          |
| 7. 風險管理與實際性    | 通過/失敗/部分 |          |
| 8. 文件與交接          | 通過/失敗/部分 |          |
| 9. MVP 後考量          | 通過/失敗/部分 |          |

### 關鍵缺失

- 列出所有必須在批准前解決的關鍵問題

### 建議

- 提供具體建議以解決每個缺失

### 最終決定

- **批准**: 計畫全面、排序適當，準備實作。
- **拒絕**: 計畫需要修訂以解決識別的缺失。
