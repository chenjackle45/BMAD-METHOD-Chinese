# {專案名稱} 資料模型

## 2. 核心應用實體 / 領域物件

{定義應用程式處理的主要物件/概念。針對每個關鍵實體重複子章節。}

### {實體名稱，例如，使用者、訂單、產品}

- **描述：** {此實體代表什麼？}
- **結構 / 介面定義：**
  ```typescript
  // 使用 TypeScript 介面作為範例
  export interface {EntityName} {
    id: string; // {描述，例如，唯一識別碼}
    propertyName: string; // {描述}
    optionalProperty?: number; // {描述}
    // ...其他屬性
  }
  ```
  _（或者，使用 JSON Schema、類別定義或其他相關格式）_
- **驗證規則：** {列出除基本型別外的任何特定驗證規則，例如，最大長度、格式、範圍。}

### {另一個實體名稱}

{...}

## API 載荷結構（如有不同）

{定義專門用於 API 傳遞或接收的資料結構，如果它們與核心實體有顯著差異。參考 `docs/api-reference.md`。}

### {API 端點 / 用途，例如，建立訂單請求}

- **結構 / 介面定義：**
  ```typescript
  // 範例
  export interface CreateOrderRequest {
    customerId: string;
    items: { productId: string; quantity: number }[];
    // ...
  }
  ```

### {另一個 API 載荷}

{...}

## 資料庫結構（如適用）

{如果使用資料庫，定義表結構或文件資料庫結構。}

### {表 / 集合名稱}

- **用途：** {此表存儲什麼資料？}
- **結構定義：**
  ```sql
  -- SQL 範例
  CREATE TABLE {TableName} (
    id VARCHAR(36) PRIMARY KEY,
    column_name VARCHAR(255) NOT NULL,
    numeric_column DECIMAL(10, 2),
    -- ...其他欄位、索引、約束
  );
  ```
  _（或者，使用 ORM 模型定義、NoSQL 文件結構等）_

### {另一個表 / 集合名稱}

{...}

## 狀態檔案結構（如適用）

{如果應用程式使用檔案來持久化狀態。}

### {狀態檔案名稱 / 用途，例如，processed_items.json}

- **用途：** {此檔案追蹤什麼狀態？}
- **格式：** {例如，JSON}
- **結構定義：**
  ```json
  {
    "type": "object",
    "properties": {
      "processedIds": {
        "type": "array",
        "items": {
          "type": "string"
        },
        "description": "已處理 ID 的列表。"
      }
      // ...其他狀態屬性
    },
    "required": ["processedIds"]
  }
  ```

## 變更記錄

| 變更     | 日期       | 版本 | 描述     | 作者        |
| -------- | ---------- | ---- | -------- | ----------- |
| 初始草稿 | YYYY-MM-DD | 0.1  | 初始草稿 | {代理/人員} |
| ...      | ...        | ...  | ...      | ...         |
