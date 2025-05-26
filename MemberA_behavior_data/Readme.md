# 會員行為分析專案

本專案旨在針對 91APP 平台特定標記為 A 的會員，進行行為資料的分析與特徵工程建構。資料來源包含 `ThreeNAPL.csv` 與 `91APP_Dataset (session 0) shop0`。

---

## 🎯 目標會員（Target Members）

- 從 `ThreeNAPL.csv` 中擷取 **第三欄標記為 A 的會員**，共 **26,891 名**。
- 從 `91APP_Dataset (session 0) shop0` 中，取出這些 A 會員的所有行為紀錄。

> 📌 補充說明：  
> 雖然行為資料的最後紀錄時間為 **2024 年 2 月**，但 `ThreeNAPL.csv` 中的標記時間為 **2024 年 9 月**，推測可能包含了新加入會員。  
> 最終分析資料中共切出 **26,868 名會員** 的行為資料。

---

## 📊 特徵設計（Feature Engineering）

| 特徵名稱                         | 說明                                                                 | 操作方法                                                                                      | 對應 function |
|----------------------------------|----------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|---------------|
| `average_product_view_mean`      | 使用者平均每個商品頁停留時間（思考時間）                            | Group by `MemberID`, `SalesPageID`，取每次 `ProductView` 最早與最晚時間差後再平均             | `productView` |
| `average_product_view_max`       | 單一商品頁面停留的最大時間                                            | 同上，取最大值                                                                                 | `productView` |
| `session_count_mean`             | 每次 session 操作次數的平均值                                        | Group by `MemberID`, `HitTime`，計算每個 session 的行為數，取平均                             | `analyze_hit_time_activity` |
| `session_count_median`           | 每次 session 操作次數的中位數                                        | 同上，取中位數                                                                                 | `analyze_hit_time_activity` |
| `session_product_count`          | 每次 session 瀏覽的商品數                                             | 待補                                                                                          | -             |
| `buy_addCart_ratio`              | 加入購物車但未購買的比率                                              | Group by `MemberID`, `SalesPageID`，分別統計加入與購買次數，取 購買 / 加入 購物車 的比例       | `buy_addCart_ratio` |
| `view_to_buy_duration_median`    | 從瀏覽到購買的中位時間（衡量衝動程度）                               | Group by `MemberID`, `SalesPageID`，統計有購買商品的最早 `ProductView` 到 `Purchase` 的時間差  | `purchase_funnel_median_times` |
| `view_to_add_duration_median`    | 從瀏覽到加入購物車的時間                                              | 統計有加入購物車商品的最早 `ProductView` 到 `AddCart` 的時間                                  | -             |
| `add_to_buy_duration_median`     | 從加入購物車到購買的時間                                              | 統計有購買商品的 `AddCart` 到 `Purchase` 的時間差                                              | -             |
| `discount_sensitivity`           | 折扣資訊對購買意願的影響                                              | 每個 session 出現下列行為時記一次：`viewecoupondetail`、`viewpromotiondetail`、`viewcoupondetail`，然後計算其購買率 | -             |
| `discount_to_purchase_ratio`     | 看折扣後實際購買的轉換率                                              | 同上                                                                                          | -             |
| `come_back`                      | 回訪次數（同商品多次出現）                                             | Group by `MemberID`, `SalesPageID`，計算同商品被瀏覽的 session 數，取平均                     | `avg_unique_hit_times_per_sale_page` |
| `purchase_randomness`            | 喜好即買的隨機程度（消費金額標準差）                                 | 統計會員實際購買商品的金額標準差                                                              | `purchase_randomness` |
| `return_ratio`                   | 退貨比例（後悔指標）                                                   | 退貨次數 / 購買次數（以主單計算）                                                              | -             |

---

## 📁 資料來源

- **`ThreeNAPL.csv`**：會員標記資料，第三欄標示 A/B 分群
- **`91APP_Dataset (session 0) shop0`**：會員行為紀錄（點擊、加入購物車、購買等）

---

## 🛠️ 產出

- `member_feature(不含退貨).csv`
  共 26,598 筆, 含 12 個 feauter (非 26,868 原因為有些會員行為資料只有一筆無法做統計分析)

---

## ✅ 待辦事項（To-Do）




## ✍️ 作者

陳星諭（Hsing-Yu Chen）  
國立臺灣大學 經濟系
