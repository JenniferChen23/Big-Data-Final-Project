# Hello BDA
針對會員（只取 A 會員）分群，看不同群體的購買決策行為和商品描述偏好，做巨量的一對一。

# 0. 事前準備
- 會員特徵 file：`?`

| ShopMemberId | 特徵 | 特徵 | 特徵 |
| --- | --- | --- | --- |
| 會員 ID | 中文, range | 中文, range | 中文, range |
| 0 | 1 | 1 | 1 |

- 商品文本斷詞 file：`SalePage_cutterms.csv`
	- 轉小寫、去除數字、去除標點、不刪除長度 = 1 的字
	- 有些商品敘述是 nan

| SalePageId | title_cutterms | desc_cutterms |
| --- | --- | --- |
| 商品 ID | 標題斷詞 | 敘述斷詞 |
| 7104874 | 戰神 mars 搖搖杯 ml 夜幕 黑 | |

- 商品文本字典 file：`SalePage_vacab.csv`

| term | freq  | source | length | is_onechar | is_english | is_number | is_special | is_punctuation |
|------|-------|--------|--------|------------|------------|-----------|------------|----------------|
| 詞 | 詞頻  | 來自標題 or 敘述 {both, desc} | 詞長 | 是否為單一個字元 | 是否為英文 | 是否為數字 | 是否為特殊符號 | 是否為 Unicode 標點符號 |
| 的   | 27380 | both   | 1      | True       | False      | False     | False      | False          |


- 商品文本 BERT file：`SalePage_bertvecs.csv`
	- 檔案太大了（1.8 GB），就先不上傳了

| SalePageId | title_bertvecs | desc_bertvecs | all_bertvecs |
| --- | --- | --- | --- |
| 商品 ID | 標題 CLS vec (768 dim)  | 敘述 CLS vec (768 dim) | 標題+敘述 CLS vec (768 dim) |
| 7104874 | [-0.54972005, 0.16861996, -0.89927554, 0.23800...] | 〃 | 〃 |

---

# 1. K-means
- 分群結果
- 分群後分析（確定群體的實際意義）
	- cluster 0
	- cluster 1
	- cluster 2
	- cluster 3

---

# 2. 不同群的商品關鍵字
- BERT vec 視覺化分不開
- TF-IDF 挑 100 個字看
	- cluster 0
	- cluster 1
	- cluster 2
	- cluster 3

Cluster cluster0 Top Keywords:

添加, 任選, 配方, 成分, 舒適, 設計, 贈品, 使用, 有效, 溫和, 
抗菌, 舒緩, 天然, 清爽, 細緻, 清潔, 日本, 隨機, 出貨, 自然, 
安心, 快速, 效果, 高效, 預防, 方便, 韓國, 台灣, 專利

Cluster cluster1 Top Keywords:

添加, 配方, 成分, 任選, 舒適, 設計, 使用, 溫和, 天然, 有效, 
贈品, 日本, 隨機, 安心, 自然, 健康, 完美, 出貨, 升級, 適用, 
快速, 長效, 親膚, 高效, 材質, 改善, 韓國, 預防, 方便, 質地, 
台灣, 技術

Cluster cluster2 Top Keywords: 衛生棉、濕紙巾、蘇菲

添加, 任選, 隨機, 贈品, 舒適, 配方, 設計, 出貨, 天然, 有效, 
安心, 成分, 透氣, 使用, 回饋, 完美, 草本, 升級, 日本, 點數, 
親膚, 認證, 全新, 健康, 自然, sgs, 通過

Cluster cluster3 Top Keywords: 價格敏感

贈品, 添加, 配方, 回饋, 安心, 隨機, 成分, 出貨, 點數, 設計, 
任選, 抗菌, 上限, 天然, 使用, 完美, 清爽, 抑菌, 升級, 有效, 
日本, 醯胺, 草本, 全新, 發送, 高效, 認證, 免運, 健康, 親膚

「入點」是啥？

- GPT、手動挑關鍵字
	- cluster 0
	- cluster 1
	- cluster 2
	- cluster 3