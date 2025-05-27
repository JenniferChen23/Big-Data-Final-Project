# 91app - K-means 結果解釋

# Cluster0 - 計畫購買型

### 數據特色

- **停留與探索時間長 → 謹慎型消費者**：
    - average_product_view_mean (0.522545) 和 average_product_view_max (1.201695) 偏高，顯示用戶在商品頁停留時間較長，可能是謹慎型消費者。session_count_mean (-0.106389) 和 session_product_count (-0.172517) 偏低，瀏覽商品數和操作次數少，表明專注於少量商品。
- **轉換與決策快 → 一旦探索完畢會立即購物**：
    - buy_addCart_ratio (-0.262507) 負值，轉換率較高，加入購物車後容易完成購買。view_to_buy_duration_median (-0.009582) 接近零，決策時間短，顯示一旦決定加入購物車，購買行為迅速。
- **折扣敏感度高→ 折扣會加速購買**：
    - discount_sensitivity (0.339324) 正值，對折扣敏感，可能因促銷活動加速購買。
- **回訪與隨機性高**：
    - come_back (0.471254) 正值，回訪次數多，可能對商品有持續興趣。purchase_randomness (0.300544) 正值，購買金額變異較大，顯示消費行為有一定隨機性。

### 消費者Persona可能是：

- 我有明確的購買標的，但我想要多方比價格及品質，雖然前面花很多時間研究，但一旦決定好我會立買完成購物的任務，相當相信自己總能選到 cp 值最好的商品。

# Cluster1 - 猶豫**型**

- **停留與探索時間長**：
    - average_product_view_mean (2.876024) 極高，average_product_view_max (1.218404) 也高，停留時間顯著長，顯示用戶非常謹慎且深入研究。session_count_mean (-0.349546) 和 session_product_count (-0.329107) 負值，瀏覽商品數少，專注於特定商品。
- **轉換與決策效率低**：
    - buy_addCart_ratio (0.132141) 正值，加入購物車後轉換率低，顯示猶豫。view_to_buy_duration_median (4.589694) 和 add_to_buy_duration_median (4.041779) 很高，決策時間極長，顯示購買過程緩慢。
- **促銷有影響**：
    - discount_sensitivity (0.438089) 正值，對折扣敏感，可能因促銷而最終購買。
- **回訪與隨機性**：
    - come_back (1.102813) 很高，回訪頻率高，顯示對商品有強烈興趣。purchase_randomness (-0.175960) 負值，購買金額變異小，顯示消費行為穩定。

### 消費者Persona可能是：

- 就算只有兩個選項也會讓我猶豫好久，常常東西加到購物車到結帳我還是會想東想西很難決策

# **Cluster 2**：衝動型購買者

- **短影音式的閱覽商品資訊：**
    - average_product_view_mean (-0.307236) 和 average_product_view_max (-0.493470) 負值，停留時間短，顯示快速瀏覽。session_count_mean (3.282144) 和 session_product_count (3.388683) 很高，瀏覽商品數多，顯示高活躍度。
- **轉換與決策快**：
    
    buy_addCart_ratio (0.036720) 正值，轉換率中等。view_to_buy_duration_median (-0.225829) 和 add_to_buy_duration_median (-0.174187) 負值，決策時間短，顯示衝動購買傾向。
    
- **促銷影響不大，買東西的快感比較大**：
    
    discount_sensitivity (-0.564445) 負值，對折扣不敏感，購買行為可能由需求驅動而非促銷。
    
- **回訪頻率與隨機性低**：
    - come_back (-0.477898) 負值，回訪次數少，可能是一次性購買。purchase_randomness (-0.019505) 負值，購買金額變異小，顯示消費行為穩定。

### 消費者Persona可能是：

- 今天心血來潮就來速逛商城有什麼，看到喜歡的，不管有沒有折扣就給他買下去

# Cluster3 - 補充必須品型購買者

- **停留與探索短**：
    - average_product_view_mean (-0.368930) 和 average_product_view_max (-0.613166) 負值，停留時間短，快速瀏覽。session_count_mean (-0.126661) 和 session_product_count (-0.101646) 負值，瀏覽商品數少，活躍度低。
- **轉換與決策快**：
    - buy_addCart_ratio (0.119860) 正值，轉換率較低。view_to_buy_duration_median (-0.192803) 和 add_to_buy_duration_median (-0.161259) 負值，決策時間短，顯示高效購買。
- **促銷影響**：
    - discount_sensitivity (-0.152251) 負值，對折扣不敏感，購買可能由明確需求驅動。
- **回訪與隨機性**：
    - come_back (0.252309) 正值，回訪次數中等，可能對某些商品有興趣。purchase_randomness (-0.137430) 負值，購買金額變異小，顯示消費穩定。

### 消費者Persona可能是：

- 商城是我家全聯拉，我老熟了大概平常有就需要這幾樣東西

# Chart

![image](https://github.com/user-attachments/assets/8911800e-3081-4579-9e5a-3b03613c4d80)

![image](https://github.com/user-attachments/assets/7d62edce-a0f8-4c2b-b13a-67e4049dcb0d)

