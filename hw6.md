# Group HomeWork_6

# 初始螢幕設計


# 系統資料架構與驗證規則 (Data Schema & Validation)
定義 AI 行銷助理與物流系統互動時的資料型態與驗證邏輯

## 1. 產品資料 (Products)
*用於 AI 判斷是否進行促銷或庫存預警*

| 欄位名稱 (Field) | 資料型態 (Data Type) | 必填 (Required) | 驗證規則 (Validation Rules) | 說明 (Description) |
| :--- | :--- | :---: | :--- | :--- |
| `product_id` | String (PK) | ✅ | Regex: `^P[0-9]{5}$` <br> (範例: P00001) | 唯一產品編號 |
| `product_name` | String | ✅ | Length: 2-100 chars | 產品名稱 |
| `category` | Enum | ✅ | [食品, 電子, 日用品, 冷凍] | 產品類別，用於行銷分眾 |
| `unit_price` | Decimal(10,2) | ✅ | Value > 0 | 單價 (新台幣) |
| `stock_qty` | Integer | ✅ | Value >= 0 | 當前庫存量 |
| `last_restock` | Date | ❌ | Format: YYYY-MM-DD | 最後進貨日 |

## 2. 客戶資料 (Customers)
*用於 AI 透過 Gmail 發送行銷資訊*

| 欄位名稱 (Field) | 資料型態 (Data Type) | 必填 (Required) | 驗證規則 (Validation Rules) | 說明 (Description) |
| :--- | :--- | :---: | :--- | :--- |
| `customer_id` | String (PK) | ✅ | Regex: `^C[0-9]{5}$` | 唯一客戶編號 |
| `company_name` | String | ✅ | Length: > 2 chars | 客戶公司或聯絡人名稱 |
| `email` | String | ✅ | Regex: `^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$` | 聯絡信箱 (Gmail Trigger 用) |
| `phone` | String | ❌ | Regex: `^09\d{8}$` or `^0\d{1,2}-\d{6,8}$` | 聯絡電話 |
| `status` | Enum | ✅ | [Active, Inactive, VIP] | 客戶等級，VIP 優先行銷 |

## 3. 行銷活動排程 (Campaigns)
*對應 n8n 中的 Google Calendar 工具*

| 欄位名稱 (Field) | 資料型態 (Data Type) | 必填 (Required) | 驗證規則 (Validation Rules) | 說明 (Description) |
| :--- | :--- | :---: | :--- | :--- |
| `campaign_id` | Integer (PK) | ✅ | Auto Increment | 活動編號 |
| `campaign_name` | String | ✅ | Length: 5-50 chars | 行銷活動主題 |
| `start_time` | DateTime | ✅ | `start_time` > Current Time | 活動開始時間 |
| `end_time` | DateTime | ✅ | `end_time` > `start_time` | 活動結束時間 |
| `target_product`| String (FK) | ✅ | 必須存在於 `Products` 表中 | 促銷主打商品 |

---
**驗證邏輯實作筆記 (For n8n Code Node):**
> 在 n8n 的 "Text Classifier" 之後，若意圖為 `create_campaign`，必須檢查 `stock_qty` 是否大於安全庫存量 (e.g., > 50)，否則 AI 應回傳「庫存不足，建議先建立進貨單」。
