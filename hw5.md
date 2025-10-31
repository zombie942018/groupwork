# Group HomeWork_5

## UML類別圖
![UML類別圖](UML類別圖.jpg)

----------------------------------
## 使用案例一：分析關鍵字趨勢(循序圖)
```mermaid
sequenceDiagram
    participant User as 使用者
    participant System as AI 助理系統
    participant API as 搜尋引擎工具 (API)

    title: UC001 - 分析關鍵字趨勢

    User->>System: 1. 輸入指令 (FR-1-1)
    activate System

    System->>System: 2. 解析指令與關鍵字
    
    System->>API: 3. 查詢關鍵字趨勢 (FR-5-1, FR-2-1)
    activate API
    
    API-->>System: 4. 回傳趨勢數據
    deactivate API
    
    System->>System: 5. 匯總 API 數據
    
    System-->>User: 6. 回報分析結果 (FR-1-3)
    deactivate System
```
----------------------------------
## 使用案例一：分析關鍵字趨勢(活動圖)
```mermaid
graph LR 
    subgraph UC001 - 分析關鍵字趨勢
        direction LR 
        
        subgraph User [使用者]
            A1(開始) --> A2[1. 輸入關鍵字指令 FR-1-1];
            A2 --> B1;
            B5 --> A3[8. 查看分析報告];
            A3 --> A4(結束);
        end

        subgraph System [AI 助理系統]
            B1[2. 解析指令與關鍵字] --> B2[3. 準備並發送 API 請求 FR-5-1];
            B2 --> C1;
            C2 --> B3[5. 匯總 API 回傳的數據];
            B3 --> B4[6. 生成結構化報告];
            B4 --> B5[7. 呈現分析結果 FR-1-3];
        end

        subgraph API [搜尋引擎工具]
            C1[4. 查詢趨勢數據 FR-2-1] --> C2[5. 回傳數據];
        end
    end
```
----------------------------------
## 使用案例二：分析競品內容(循序圖)
```mermaid
sequenceDiagram
    participant User as 使用者
    participant System as AI 助理系統
    participant Crawler as 網頁爬蟲工具

    title: UC002 - 分析競品內容

    User->>System: 1. 輸入競品分析指令 (FR-1-1)
    activate System

    System->>System: 2. 解析指令 (任務, URL, 時間)
    
    System->>Crawler: 3. 啟動爬蟲 (FR-5-2)
    activate Crawler
    
    Crawler-->>System: 4a. 回傳網頁原始內容
    deactivate Crawler
    
    System->>System: 4b. 過濾內容並執行 NLP 分析 (FR-2-2)
    System->>System: 5. 匯總分析結果
    
    System-->>User: 6. 回報競品分析報告 (FR-1-3)
    deactivate System
```
----------------------------------
## 使用案例二：分析競品內容(活動圖)
```mermaid
graph LR
    subgraph UC002 - 分析競品內容
        direction LR
        
        subgraph User [使用者]
            A1(開始) --> A2[1. 輸入競品分析指令 FR-1-1];
            A2 --> B1;
            B5 --> A3[7. 查看分析報告];
            A3 --> A4(結束);
        end

        subgraph System [AI 助理系統]
            B1[2. 解析指令 如：URL,時間] --> B2[3. 準備並啟動爬蟲 FR-5-2];
            B2 --> C1;
            C2 --> B3[5. 執行 NLP 分析 FR-2-2];
            B3 --> B4[6. 生成結構化報告];
            B4 --> B5[7. 呈現分析結果 FR-1-3];
        end

        subgraph Crawler [網頁爬蟲工具]
            C1[4. 爬取目標網站內容] --> C2[5. 回傳網頁內容];
        end
    end
```
----------------------------------
## 使用案例三：生成內容草稿(循序圖)
```mermaid
sequenceDiagram
    participant User as 使用者
    participant System as AI 助理系統
    participant API as 搜尋引擎工具 (API)

    title: UC003 - 生成內容草稿

    User->>System: 1. 輸入內容生成指令 (FR-1-1)
    activate System

    System->>System: 2. 解析任務 (主題, Persona)
    
    System->>API: 3. 搜尋相關資料 (FR-5-1, FR-2-1)
    activate API
    
    API-->>System: 4a. 回傳搜尋結果
    deactivate API
    
    System->>System: 4b. 生成內容大綱 (FR-3-1)
    
    System-->>User: 5. 呈現大綱並請求確認 (FR-1-2)
    
    User->>System: 6. 回覆確認大綱
    
    System->>System: 7. 撰寫完整文章草稿 (FR-3-2)
    
    System-->>User: 8. 呈現最終草稿 (FR-1-3)
    deactivate System
```
----------------------------------
## 使用案例三：生成內容草稿(活動圖)
```mermaid
graph LR
    subgraph UC003 - 生成內容草稿
        direction LR
        
        subgraph User [使用者]
            A1(開始) --> A2[1. 輸入內容生成指令 FR-1-1];
            A2 --> B1;
            B4 --> A3[6. 回覆確認大綱];
            A3 --> B5;
            B7 --> A4[9. 審閱草稿];
            A4 --> A5(結束);
        end

        subgraph System [AI 助理系統]
            B1[2. 解析任務 如：主題, Persona] --> B2[3. 準備並發送 API 請求 FR-5-1];
            B2 --> C1;
            C2 --> B3[5. 生成內容大綱 FR-3-1];
            B3 --> B4[6. 呈現大綱並請求確認 FR-1-2];
            B5[7. 根據大綱與 Persona 撰寫草稿 FR-3-2] --> B6;
            B6[8. 呈現完整草稿 FR-1-3] --> B7;
        end

        subgraph API [搜尋引擎工具]
            C1[4. 搜尋相關資料 FR-2-1] --> C2[5. 回傳搜尋結果];
        end
    end
```
----------------------------------
