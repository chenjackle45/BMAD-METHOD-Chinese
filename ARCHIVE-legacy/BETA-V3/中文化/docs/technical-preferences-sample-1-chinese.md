# 我的企業級 Go 與事件驅動架構實戰原則

這不僅僅是一份清單；而是我們在 AWS 上以 Golang 建構可擴展、高韌性的電商系統，並以 Kafka 作為事件主幹時，所驗證有效的精華。如果我們要打造系統，這就是行動手冊。

## 核心架構理念

- **Pattern:** Microservices Architecture

  - **我的立場：** 對於我們的電商領域來說絕對必要。針對新的重大能力，這是不可協商的。
  - **原因：** 需要讓服務能夠獨立擴展，避免單體瓶頸。這對團隊自主與速度至關重要。
  - **關鍵信條：** 服務 _必須_ 對應明確的業務能力。Loose coupling 不是流行語，而是硬性需求。

- **Pattern:** Event-Driven Architecture (EDA) with Kafka

  - **我的立場：** 作為服務間通訊及任何嚴肅非同步工作的預設選擇。
  - **原因：** 這是我們實現韌性與擴展性的方式。透過事件解耦對系統演進至關重要。
  - **關鍵信條：**
    - Kafka 作為核心事件流。這裡絕不能省略。
    - AWS SNS/SQS 適合較簡單、輔助型事件或特定 Lambda glue，但業務事件還是 Kafka 為王。
    - Schema Registry（Confluent 或 AWS Glue）是**強制性**的。沒有 schema 就不能合併。這能避免許多後續問題。

- **Pattern:** Hexagonal Architecture (Ports and Adapters)

  - **我的立場：** 強烈推薦用於我們 Go 服務的結構設計。
  - **原因：** 能讓領域邏輯保持純粹，免受 HTTP、資料庫或訊息細節干擾。可測性大幅提升。

- **Pattern:** API First Design

  - **我的立場：** 強制要求。在撰寫任何實作程式碼前，先設計 API。
  - **原因：** 明確契約能大幅減少整合痛苦。OpenAPI v3+ 是我們的共同語言。
  - **關鍵信條：** API 文件必須由這些規格自動產生，且永遠保持最新。

- **Pattern:** CQRS (Command Query Responsibility Segregation)
  - **我的立場：** 是工具箱中強大的工具，但不是萬用解決方案。
  - **原因：** 對於讀取量大或寫讀模型明顯分離的服務非常適合。
  - **關鍵信條：** 謹慎應用。它會增加複雜度，因此必須明確且具體帶來效益。

## 我的技術棧預設

- **Category:** Cloud Provider

  - **Technology:** AWS (Amazon Web Services)
  - **我的立場：** 我們的策略平台，全力投入。
  - **原因：** 企業級深度投資、服務成熟，並具備我們所需的規模。
  - **常用 AWS 服務：**
    - Compute：AWS Lambda 是大多數 microservices 的主力。若 Lambda 受限（長時間任務、特殊需求）則用 AWS Fargate。
    - Messaging：Apache Kafka（優先選用 MSK 代管，若需 MSK 無法提供的細節才自架 EC2）。SQS/SNS 用於 Lambda DLQ、簡單 fan-out。
    - Database：
      - NoSQL：DynamoDB 是高吞吐服務首選。其擴展性與代管特性極具優勢。
      - Relational：當資料模型確實為關聯式或需複雜交易時選用 RDS PostgreSQL，DynamoDB 處理起來會較彆扭。
      - Caching：ElastiCache for Redis。標準配置。
    - API Management：API Gateway。負責 REST 與 HTTP API 的入口。
    - Storage：S3。從靜態資產、日誌到資料湖基礎皆適用。
    - Identity：IAM 用於服務角色，若需處理客戶端認證則用 Cognito。
    - Observability：CloudWatch（Logs、Metrics、Alarms）及 AWS X-Ray 進行分散式追蹤。不可或缺。
    - Schema Management：AWS Glue Schema Registry 或 Confluent。擇一並統一使用。
    - Container Registry：ECR。

- **Category:** Backend Language & Runtime

  - **Technology:** Golang
  - **我的立場：** 新 microservice 開發的主力骨幹。
  - **目標版本：** 採用最新穩定版（如 1.21+），但每個專案的 `go.mod` 會鎖定具體 minor 版本。
  - **原因：** 簡潔、效能卓越、一流的並發能力，完美契合雲原生。精簡高效，特別適合 Lambda。
  - **常用 Go 函式庫（專案內皆鎖定版本）：**
    - HTTP：`chi` 因組合性佳常為首選，但 `gin-gonic` 也很穩。若極簡則用標準 `net/http`。
    - Config：`viper`。
    - Logging：`uber-go/zap` 或 `rs/zerolog`，用於結構化日誌。必須輸出 JSON。
    - Kafka：`confluent-kafka-go` 穩定可靠，`segmentio/kafka-go` 為替代方案。
    - AWS：`aws-sdk-go-v2`。
    - Testing：Go 內建 `testing` 很好用，`testify/assert` 與 `testify/mock` 為標配。資料庫 schema 用 `golang-migrate`。
    - Data/SQL：避免使用重量級 ORM，`sqlx` 通常已足夠。`sqlc` 可從 SQL 產生型別安全的 Go 程式碼很優秀。DynamoDB 則直接用 SDK。

- **Category:** Data Serialization
  - **Format:** Protobuf
    - **我的立場：** Kafka 訊息與所有 gRPC 通訊的標準格式。
    - **原因：** 效能佳、支援 schema 演進、強型別極具價值。
  - **Format:** JSON
    - **我的立場：** 所有 REST API 載荷（請求/回應）的標準格式。
    - **原因：** 通用且易於閱讀。

## 我的設計與風格指引

- **API Design (RESTful):**

  - **我的立場：** 同步 API 的主要風格。
  - **關鍵信條：**
    - 嚴格遵循 HTTP 語意。正確的方法、正確的狀態碼。
    - 資源名稱用複數名詞。URL 保持簡潔直觀。
    - 錯誤回應標準化。無例外。
    - 服務必須無狀態。
    - `PUT`/`DELETE` 必須具冪等性，`POST` 亦應如此（若適用）。
    - 透過 URI 進行版本控管（如 `/v1/...`），簡單明瞭。
    - 認證：外部用 OAuth 2.0（Client Credentials、Auth Code），內部用 JWT。API Gateway authorizers 是好幫手。

- **Infrastructure as Code (IaC):**

  - **Tool:** HashiCorp Terraform
  - **我的立場：** 我們唯一的基礎設施管理方式。
  - **原因：** 宣告式、經過實戰驗證，且我們已建立完善的模組庫。
  - **關鍵信條：** Terraform 模組必須可重用。狀態存於 S3 並以 DynamoDB 鎖定。

- **Concurrency (Golang):**

  - **我的立場：** 善用 Go 的優勢，但需有紀律。
  - **關鍵信條：** Goroutine 與 channel 強大但要明智使用。用 worker pool 控制並發。所有服務必須支援優雅關閉。

- **Error Handling (Golang):**

  - **我的立場：** Go 明確的錯誤處理是特點而非缺陷，應充分利用。
  - **關鍵信條：** 一律回傳錯誤。用 `fmt.Errorf` 搭配 `%w`（或函式庫）包裝錯誤以增加脈絡。記錄錯誤時要有 correlation ID。明確區分可重試與致命錯誤。

- **Testing:**

  - **我的立場：** 絕對必要且是開發不可分割的一部分，絕非事後補救。
  - **關鍵信條：**
    - Unit Tests：涵蓋所有關鍵邏輯。外部依賴一律 mock。
    - Integration Tests：驗證互動（如服務與資料庫、服務與 Kafka）。Testcontainers 或 localstack 在此極有價值。
    - Contract Tests (Events)：特別針對 Kafka。確保 producer 與 consumer 在執行前就 schema 達成一致。
    - E2E Tests：針對關鍵業務流程，API 驅動。

- **Observability:**

  - **我的立場：** 無法觀測就無法負責。
  - **關鍵信條：** 結構化 JSON 日誌。全流程貫穿 correlation ID。主要營運指標（速率、錯誤、延遲、消費者延遲）透過 CloudWatch 監控。AWS X-Ray 進行追蹤。

- **Security:**

  - **我的立場：** 安全性必須內建，而非事後補強。每個人都要負責。
  - **關鍵信條：** 所有 IAM 角色皆採最小權限。Secrets 存於 AWS Secrets Manager。所有邊界嚴格輸入驗證。自動化漏洞掃描（Go modules、容器）。靜態分析（`go vet`、`staticcheck`、`gosec`）。

- **Development Workflow:**
  - **我的立場：** 流暢、自動化且可預測。
  - **關鍵信條：** Git 為基本。Trunk-Based Development 通常是 microservices 首選。CI/CD（GitHub Actions、GitLab CI 或 Jenkins，依企業標準）必須健全：lint、靜態分析、各類測試、建置、部署。
  - Docker 用於本地開發一致性與 Fargate。Lambda 則用 zip 套件。

## 關鍵橫向實踐

- **Configuration Management:**

  - **我的立場：** 程式碼中絕不可有 magic string 或硬編碼設定。
  - **關鍵信條：** 透過環境變數、AWS Parameter Store 或 AppConfig 外部化設定。設定需依環境區分。

- **Resiliency & Fault Tolerance:**
  - **我的立場：** 為失敗而設計，因為它 _一定_ 會發生。
  - **關鍵信條：** 對暫時性問題進行重試（需指數退避與抖動）。對不穩定依賴設計 circuit breaker。Kafka 無法處理的訊息進 DLQ。所有地方都要有合理 timeout。
