title: System Overview
---

# 2. System Overview

ATP Project 的整體系統架構（Architecture）是由以下五個部分所組成：

1. [頻道 Channel Layer](#2-1-Channel-Layer)
2. [價格引擎 Pricing Engine Layer](#2-2-Pricing-Engine-Layer)
3. [演算法 Algorithm Layer](#2-3-Algorithm-Layer)
4. [訂單管理器 Order Manager Layer](#2-4-Order-Manager-Layer)
5. [資料庫 Database Layer](#2-5-Database-Layer)

<hr/>
## 2.1. Channel Layer

交易所（Exchange）會提供 API 讓我們取得金融市場數據的串流資料，頻道層（Channel Layer）的目的就是要不斷向交易所傳送合格的價格請求（Valid Request），並將資料送入價格引擎（Pricing Engine）中，請留意兩件事：
Inputs：一個頻道可能請求了兩個以上的交易所的資料
例如黃金價格的頻道可能除了有黃金之外，也有黃金的 ETF（指數型基金）
Outputs：一個頻道可能跳過價格引擎直接被演算法（Algo）使用
由於價格引擎的設計中可能會將市場數據以固定頻率推送到 Redis 中，所以演算法可能會有跳過 Redis 並且不想受價格引擎的推送頻率限制的需求

<hr/>
## 2.2. Pricing Engine Layer

價格引擎（Pricing Engine）主要是要將訂閱的頻道資料（通常是 JSON 格式）做輕量的處理，並加入到 Redis 中，同時記錄與管理引擎本身的效能與速度
輕量處理包含（嚴格來說不只有價格）：

### 2.2.1. 價格驗證

1. 首先要先對歷史價格變化進行計算，並算出這些變化的信賴區間
2. 根據需求與設定，若有價格變化超過信賴區間，就拒絕將價格推入 Redis
3. 將極端資料進行記錄與回報

### 2.2.2. 價格整理

將多個報價、多個頻道的價格處理成一個價格

TBD

### 2.2.3. 價格推送

根據需求將資料依照一頻率推送到 Redis 中

TBD

<hr/>
## 2.3. Algorithm Layer

TBD

<hr/>
## 2.4. Order Manager Layer

TBD

<hr/>
## 2.5. Database Layer

TBD