# Group HomeWork_6
# 初始套件設計
![Telegram行程](Telegram行程.jpg)
![今日摘要](今日摘要.jpg)
![語音輸入排定行程](語音輸入排定行程.jpg)
# 使用畫面
![初始螢幕設計](Telegram1.jpg)
![初始螢幕設計](Telegram2.jpg)
# 理想
```mermaid
graph LR
    %% 定義樣式
    classDef trigger fill:#ff6d5a,stroke:#333,stroke-width:2px,color:white;
    classDef process fill:#5a91ff,stroke:#333,stroke-width:2px,color:white;
    classDef ai fill:#9d6dff,stroke:#333,stroke-width:4px,color:white;
    classDef tool fill:#00c853,stroke:#333,stroke-width:2px,color:white;
    classDef output fill:#ffcc00,stroke:#333,stroke-width:2px,color:black;

    %% --- 輸入層 (螢幕介面 1 & 2) ---
    subgraph Input_Layer [輸入端：使用者與客戶介面]
        TG_Trigger[("Telegram Trigger<br>(指揮中心)<br>Input: 指令/查詢")]:::trigger
        GM_Trigger[("Gmail Trigger<br>(監控台)<br>Input: 客戶信件")]:::trigger
    end

    %% --- 處理層 ---
    subgraph Process_Layer [核心處理：意圖識別與決策]
        Classifier[["Text Classifier<br>(意圖分類)"]]:::process
        Agent{{"AI Agent<br>(行銷助理大腦)<br>Model: Gemini"}}:::ai
        
        %% 模擬資料庫讀取
        DB_Sim[("模擬 DB<br>讀取: 客戶資料表<br>Check: Member/VIP")]:::process
    end

    %% --- 輸出/工具層 (螢幕介面 3 & 匯出檔案) ---
    subgraph Output_Layer [執行端：匯出檔案與結果]
        Cal_Tool["Google Calendar Tool<br>(建立排程)"]:::tool
        Mail_Tool["Gmail Send Tool<br>(建立草稿)"]:::tool
        TG_Reply["Telegram Reply<br>(回報結果)"]:::tool
        
        %% 匯出物件與欄位
        Out_Event[/"匯出: 行銷活動<br>Fields: Title, StartTime,<br>Location, Description"/]:::output
        Out_Draft[/"匯出: 行銷郵件<br>Fields: To, Subject,<br>Body, Attachment"/]:::output
        Out_Log[/"匯出: 執行報告<br>Fields: Status, Msg"/]:::output
    end

    %% --- 連線關係 ---
    TG_Trigger --> Classifier
    GM_Trigger --> Classifier
    Classifier -- "意圖: 行銷/查詢" --> Agent
    
    Agent -.-> DB_Sim
    DB_Sim -.-> Agent
    
    Agent -- "Tool: Schedule" --> Cal_Tool
    Agent -- "Tool: Draft Email" --> Mail_Tool
    Agent -- "Final Answer" --> TG_Reply
    
    Cal_Tool --> Out_Event
    Mail_Tool --> Out_Draft
    TG_Reply --> Out_Log
```

# AI Agent 資料型態 & 驗證規則

定義 AI 行銷助理在處理輸入資料（來自 Telegram 指令或 Gmail 觸發）與後端系統互動時的資料規格。

## 1. 會員/客戶資料 (Member / Customer)
*用途：用於 n8n Gmail Node 發送行銷信件與 Google Contacts 聯絡人同步*

| 欄位名稱 (Field) | 資料型態 (Type) | 必填 | 驗證規則 (Validation Rules) | 說明 (Description) |
| :--- | :--- | :---: | :--- | :--- |
| `customer_id` | String (PK) | ✅ | Regex: `^C\d{5}$` <br> (例: C00123) | 系統唯一識別碼 |
| `name` | String | ✅ | Length: 2-50 chars | 客戶名稱或公司行號 |
| `email` | String | ✅ | Regex: `^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$` | **[關鍵]** 行銷信件發送目標 |
| `phone` | String | ❌ | Regex: `^09\d{8}$` | 手機號碼，用於簡訊通知 |
| `purchase_tier` | Enum | ✅ | [`General`, `VIP`, `Wholesale`] | 客戶分級，影響 AI 推薦策略 |

## 2. 行銷活動排程 (Marketing Campaign)
*用途：對應 n8n Google Calendar Tool 建立的活動物件*

| 欄位名稱 (Field) | 資料型態 (Type) | 必填 | 驗證規則 (Validation Rules) | 說明 (Description) |
| :--- | :--- | :---: | :--- | :--- |
| `campaign_title` | String | ✅ | Format: `[行銷] {Subject}` | 行事曆標題格式 |
| `start_datetime` | ISO8601 | ✅ | > `Current_Timestamp` | 活動開始時間 |
| `end_datetime` | ISO8601 | ✅ | > `start_datetime` | 活動結束時間 |
| `attendees` | List<String> | ❌ | Valid Email Format | 參與行銷會議的人員 Email |
| `description` | Text | ❌ | Max Length: 500 | AI 生成的活動摘要 |

---
