# Product Owner (PO) 驗證檢查清單

本檢查清單作為 Product Owner 在開發執行前，驗證完整 MVP 規劃的全面性框架。PO 應系統性地逐項檢查，記錄符合狀態並註明任何缺失。

## 1. PROJECT SETUP & INITIALIZATION

### 1.1 Project Scaffolding

- [ ] Epic 1 包含專案建立／初始化的明確步驟
- [ ] 若使用 starter template，包含 clone／設置步驟
- [ ] 若從零開始，所有必要的 scaffolding 步驟均已定義
- [ ] 已包含初始 README 或文件設置
- [ ] 已定義 repository 設置與初始 commit 流程（如適用）

### 1.2 Development Environment

- [ ] 已明確定義本地開發環境設置
- [ ] 已指定所需工具及版本（如 Node.js、Python 等）
- [ ] 已包含安裝相依套件的步驟
- [ ] 已處理設定檔（dotenv、config files 等）
- [ ] 已包含開發伺服器設置

### 1.3 Core Dependencies

- [ ] 所有關鍵套件／函式庫於流程初期即安裝
- [ ] 已妥善處理套件管理（npm、pip 等）
- [ ] 已適當定義版本規格
- [ ] 已註明相依衝突或特殊需求

## 2. INFRASTRUCTURE & DEPLOYMENT SEQUENCING

### 2.1 Database & Data Store Setup

- [ ] 在進行任何資料庫操作前已選定／設置資料庫
- [ ] 在資料操作前已建立 schema 定義
- [ ] 如適用，已定義 migration 策略
- [ ] 如有需要，已包含 seed data 或初始資料設置
- [ ] 早期已建立資料庫存取模式與安全性

### 2.2 API & Service Configuration

- [ ] 在實作 endpoint 前已設置 API framework
- [ ] 在實作服務前已建立 service architecture
- [ ] 在設置受保護路由前已設置 authentication framework
- [ ] 在使用前已建立 middleware 與共用工具

### 2.3 Deployment Pipeline

- [ ] 在進行任何部署動作前已建立 CI/CD pipeline
- [ ] 在使用前已設置 Infrastructure as Code (IaC)
- [ ] 早期已定義環境設定（dev、staging、prod）
- [ ] 在實作前已定義部署策略
- [ ] 已處理 rollback 流程或相關考量

### 2.4 Testing Infrastructure

- [ ] 在撰寫測試前已安裝測試框架
- [ ] 測試環境設置於測試實作前完成
- [ ] 在測試前已定義 mock services 或資料
- [ ] 在使用前已建立測試工具或輔助程式

## 3. EXTERNAL DEPENDENCIES & INTEGRATIONS

### 3.1 Third-Party Services

- [ ] 已明確標示所需服務的帳號建立步驟
- [ ] 已定義 API key 取得流程
- [ ] 已包含安全儲存憑證的步驟
- [ ] 已考慮 fallback 或離線開發選項

### 3.2 External APIs

- [ ] 已明確標示與外部 API 的整合點
- [ ] 已正確安排與外部服務的認證流程
- [ ] 已注意 API 限制或約束
- [ ] 已考慮 API 失效時的備援策略

### 3.3 Infrastructure Services

- [ ] 雲端資源配置流程安排正確
- [ ] 已標示 DNS 或網域註冊需求
- [ ] 如有需要，已包含 Email 或訊息服務設置
- [ ] CDN 或靜態資產託管設置於使用前完成

## 4. USER/AGENT RESPONSIBILITY DELINEATION

### 4.1 User Actions

- [ ] 僅將需要人工介入的責任分配給使用者
- [ ] 外部服務帳號建立正確分配給使用者
- [ ] 購買或付款行為正確分配給使用者
- [ ] 憑證提供正確分配給使用者

### 4.2 Developer Agent Actions

- [ ] 所有程式碼相關任務分配給 developer agent
- [ ] 自動化流程正確歸屬於 agent 責任
- [ ] 設定管理正確分配
- [ ] 測試與驗證分配給適當 agent

## 5. FEATURE SEQUENCING & DEPENDENCIES

### 5.1 Functional Dependencies

- [ ] 依賴其他功能的 feature 排序正確
- [ ] 共用元件於使用前先建置
- [ ] 使用者流程遵循合乎邏輯的順序
- [ ] 認證功能於受保護路由／功能前完成

### 5.2 Technical Dependencies

- [ ] 低階服務於高階服務前建置
- [ ] 函式庫與工具於使用前建立
- [ ] 資料模型於操作前定義
- [ ] API endpoint 於 client 使用前定義

### 5.3 Cross-Epic Dependencies

- [ ] 後續 epic 建立於前面 epic 的功能之上
- [ ] 無 epic 依賴於後續 epic 的功能
- [ ] 早期 epic 建立的基礎設施持續被利用
- [ ] 維持漸進式價值交付

## 6. MVP SCOPE ALIGNMENT

### 6.1 PRD Goals Alignment

- [ ] PRD 中所有核心目標於 epic／story 中均有覆蓋
- [ ] 功能直接支持所定義的 MVP 目標
- [ ] 未包含超出 MVP 範疇的多餘功能
- [ ] 關鍵功能有適當優先順序

### 6.2 User Journey Completeness

- [ ] 所有關鍵使用者旅程皆完整實作
- [ ] 已處理邊界情境與錯誤場景
- [ ] 已納入使用者體驗考量
- [ ] 如有規範，已納入無障礙需求

### 6.3 Technical Requirements Satisfaction

- [ ] 已處理 PRD 中所有技術限制
- [ ] 已納入非功能性需求
- [ ] 架構決策符合指定限制
- [ ] 已適當處理效能考量

## 7. RISK MANAGEMENT & PRACTICALITY

### 7.1 Technical Risk Mitigation

- [ ] 複雜或不熟悉技術有對應學習／原型 story
- [ ] 高風險元件有明確驗證步驟
- [ ] 風險整合有備援策略
- [ ] 效能疑慮有明確測試／驗證

### 7.2 External Dependency Risks

- [ ] 已認知並減緩第三方服務風險
- [ ] 已處理 API 限制或約束
- [ ] 關鍵外部服務有備援策略
- [ ] 已考慮外部服務的成本影響

### 7.3 Timeline Practicality

- [ ] Story 複雜度與排序反映合理時程
- [ ] 已最小化或管理對外部因素的依賴
- [ ] 盡可能啟用平行作業
- [ ] 已識別並優化關鍵路徑

## 8. DOCUMENTATION & HANDOFF

### 8.1 Developer Documentation

- [ ] API 文件隨實作同步建立
- [ ] 安裝說明完整
- [ ] 架構決策有文件記錄
- [ ] 設計模式與慣例有文件記錄

### 8.2 User Documentation

- [ ] 如有需要，包含使用者指南或說明文件
- [ ] 已考慮錯誤訊息與使用者回饋
- [ ] Onboarding 流程完整規劃
- [ ] 如適用，已定義支援流程

## 9. POST-MVP CONSIDERATIONS

### 9.1 Future Enhancements

- [ ] 明確區分 MVP 與未來功能
- [ ] 架構支援規劃中的未來擴充
- [ ] 已記錄技術債考量
- [ ] 已標示可擴充點

### 9.2 Feedback Mechanisms

- [ ] 如有需要，已納入分析或使用追蹤
- [ ] 已考慮使用者回饋收集
- [ ] 已處理監控與警示
- [ ] 已納入效能量測

## VALIDATION SUMMARY

### Category Statuses

| Category                                  | Status            | Critical Issues |
| ----------------------------------------- | ----------------- | --------------- |
| 1. Project Setup & Initialization         | PASS/FAIL/PARTIAL |                 |
| 2. Infrastructure & Deployment Sequencing | PASS/FAIL/PARTIAL |                 |
| 3. External Dependencies & Integrations   | PASS/FAIL/PARTIAL |                 |
| 4. User/Agent Responsibility Delineation  | PASS/FAIL/PARTIAL |                 |
| 5. Feature Sequencing & Dependencies      | PASS/FAIL/PARTIAL |                 |
| 6. MVP Scope Alignment                    | PASS/FAIL/PARTIAL |                 |
| 7. Risk Management & Practicality         | PASS/FAIL/PARTIAL |                 |
| 8. Documentation & Handoff                | PASS/FAIL/PARTIAL |                 |
| 9. Post-MVP Considerations                | PASS/FAIL/PARTIAL |                 |

### Critical Deficiencies

- 列出所有必須在核准前解決的重大問題

### Recommendations

- 針對每項缺失提供具體建議

### Final Decision

- **APPROVED**：規劃內容完整、排序正確，可進入實作階段。
- **REJECTED**：規劃需修正以解決已識別之缺失。
