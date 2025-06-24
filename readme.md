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
	- cluster 0: 8043 會員, 7485 商品
	- cluster 1: 16790 會員, 7123 商品
	- cluster 2: 1013 會員, 1072 商品
	- cluster 3: 752 會員, 509 商品
- 分群後分析（依照數據確定群體的實際意義）
	- cluster 0: 計畫購買型
	- cluster 1: 補充必須品型購買者
	- cluster 2: 衝動型購買者
	- cluster 3: 猶豫型
- 分群後分析（GPT 依照 TF-IDF 猜的）

| 群組        | 關鍵描述                   | 潛在會員類型                       |
| --------- | ---------------------- | ---------------------------- |
| Cluster 0 | 天然、溫和、修護、精華、成分導向       | 敏感肌護膚族：重視成分安全、天然、有修護需求者      |
| Cluster 1 | 臉部+頭髮保養、香氛、設計感         | 品質導向型：講究整體外表與氣味、品牌、設計的女性     |
| Cluster 2 | 衛生棉、吸收、防漏、點數、蘇菲等促銷與功能詞 | 實用型消費者：以女性生理用品為主、重視舒適與回饋     |
| Cluster 3 | 贈品、包裝、入點、保濕、精華         | 價格敏感族：關注優惠與行銷活動、喜歡包裝與贈品的年輕族群 |

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
安心, 快速, 入點, 效果, 高效, 預防, 方便, 韓國, 台灣, 專利

Cluster cluster1 Top Keywords:

添加, 配方, 成分, 任選, 舒適, 設計, 使用, 溫和, 天然, 有效, 
贈品, 日本, 隨機, 安心, 自然, 健康, 完美, 出貨, 升級, 適用, 
快速, 長效, 親膚, 高效, 材質, 改善, 韓國, 預防, 方便, 質地, 
台灣, 技術, 入點

Cluster cluster2 Top Keywords: 衛生棉、濕紙巾、蘇菲

添加, 任選, 入點, 隨機, 贈品, 舒適, 配方, 設計, 出貨, 天然, 
有效, 安心, 成分, 透氣, 使用, 回饋, 完美, 草本, 升級, 日本, 
點數, 親膚, 認證, 全新, 健康, 自然, sgs, 通過

Cluster cluster3 Top Keywords: 價格敏感

入點, 贈品, 添加, 配方, 回饋, 安心, 隨機, 成分, 出貨, 點數, 
設計, 任選, 抗菌, 上限, 天然, 使用, 完美, 清爽, 抑菌, 升級, 
有效, 日本, 醯胺, 草本, 全新, 發送, 高效, 認證, 免運, 健康, 
親膚

「入點」是啥？回饋入點

- GPT（依照人工描述猜字）、手動挑關鍵字（約比其他群兩倍多）
	- cluster 0 計畫購買型: 限時, 耐用, emoji, 買一送一, 規格, 限量, 實用
	- cluster 1 補充必須品型購買者: 限時, 耐用, emoji, 限量, 規格, 買一送一, 好用, 實用
	- cluster 2 衝動型購買者: 贈, 安心, 抽, 認證, 回饋, SGS, 點數, 回購
	- cluster 3 猶豫型: 贈, 安心, 送, 回饋, 點數, 免運, 首選, 實測, 必買,  優惠, 折扣, 促銷, 超有感, 品牌比他群少
- GPT、手動挑關鍵字（比例細節）
	- cluster 0: 限時(2.44%), 耐用(1.24%), 買一送一(0.59%), 規格(0.49%), 限量(0.47%), 實用(0.21%)
	- cluster 1: 限時(2.11%), 耐用(1.19%), 限量(0.56%), 規格(0.46%), 買一送一(0.42%), 好用(0.37%), 實用(0.22%)
	- cluster 2: 贈(10.07%), 安心(7.37%), 抽(6.16%), 認證(3.26%), 回饋(2.61%), SGS(2.33%), 點數(2.24%), 回購(0.09%)
	- cluster 3: 贈(17.49%), 安心(9.04%), 送(4.52%), 回饋(4.13%), 點數(3.93%), 免運(1.38%), 首選(0.39%), 實測(0.39%), 必買(0.20%),  優惠(0.20%), 折扣(0.20%), 促銷(0.20%), 超有感(0.20%), 品牌比他群少(0.79%)


'滿' 在不同 Cluster 的比例:
cluster0: 4.41%
cluster1: 4.27%
cluster2: 6.53%
cluster3: 10.22%

'限時特賣'=='特賣' 在不同 Cluster 的比例:
cluster0: 2.38%
cluster1: 2.01%
cluster2: 1.87%
cluster3: 0.98%

### emoji
emoji 在不同 Cluster 的比例:
cluster0: 0.79%
cluster1: 0.81%
cluster2: 0.47%
cluster3: 0.39%

### Cluster0 計畫購買型, 重視性價比、品質、比價資訊
'耐用' 在不同 Cluster 的比例:
cluster0: 1.24%
cluster1: 1.19%
cluster2: 0.47%
cluster3: 0.39%

'買一送一' 在不同 Cluster 的比例:
cluster0: 0.59%
cluster1: 0.42%
cluster2: 0.19%
cluster3: 0.20%

'限時' 在不同 Cluster 的比例:
cluster0: 2.44%
cluster1: 2.11%
cluster2: 1.87%
cluster3: 1.18%

'實用' 在不同 Cluster 的比例:
cluster0: 0.21%
cluster1: 0.22%
cluster2: 0.00%
cluster3: 0.00%

'規格' 在不同 Cluster 的比例:
cluster0: 0.49%
cluster1: 0.46%
cluster2: 0.37%
cluster3: 0.20%

### Cluster3(舊版 Cluster1) 猶豫型, 需要安全感、保證、信任感、社會證明
'回饋' 在不同 Cluster 的比例:
cluster0: 1.28%
cluster1: 1.00%
cluster2: 2.61%
cluster3: 4.13%

'贈' 在不同 Cluster 的比例:
cluster0: 6.63%
cluster1: 5.57%
cluster2: 10.07%
cluster3: 17.49%

'免運' 在不同 Cluster 的比例:
cluster0: 0.20%
cluster1: 0.18%
cluster2: 0.37%
cluster3: 1.38%

'必買' 在不同 Cluster 的比例:
cluster0: 0.08%
cluster1: 0.06%
cluster2: 0.09%
cluster3: 0.20%

'首選' 在不同 Cluster 的比例:
cluster0: 0.25%
cluster1: 0.29%
cluster2: 0.19%
cluster3: 0.39%

'安心' 在不同 Cluster 的比例:
cluster0: 5.33%
cluster1: 5.52%
cluster2: 7.37%
cluster3: 9.04%

'優惠' 在不同 Cluster 的比例:
cluster0: 0.04%
cluster1: 0.04%
cluster2: 0.00%
cluster3: 0.20%

'折扣' 在不同 Cluster 的比例:
cluster0: 0.01%
cluster1: 0.01%
cluster2: 0.00%
cluster3: 0.20%

'促銷' 在不同 Cluster 的比例:
cluster0: 0.01%
cluster1: 0.03%
cluster2: 0.09%
cluster3: 0.20%

'品牌' 在不同 Cluster 的比例:
cluster0: 1.28%
cluster1: 1.36%
cluster2: 1.40%
cluster3: 0.79%

'實測' 在不同 Cluster 的比例:
cluster0: 0.17%
cluster1: 0.21%
cluster2: 0.19%
cluster3: 0.39%

'超有感' 在不同 Cluster 的比例:
cluster0: 0.08%
cluster1: 0.10%
cluster2: 0.09%
cluster3: 0.20%

'點數' 在不同 Cluster 的比例:
cluster0: 0.92%
cluster1: 0.86%
cluster2: 2.24%
cluster3: 3.93%

'送' 在不同 Cluster 的比例:
cluster0: 2.44%
cluster1: 3.10%
cluster2: 3.17%
cluster3: 4.52%

### Cluster2 衝動型, 喜歡新奇、直覺打動、即時感受、情緒刺激

'衛生紙' 在不同 Cluster 的比例:
cluster0: 0.25%
cluster1: 0.29%
cluster2: 0.75%
cluster3: 0.39%

'回購' 在不同 Cluster 的比例:
cluster0: 0.05%
cluster1: 0.06%
cluster2: 0.09%
cluster3: 0.00%

'抽' 在不同 Cluster 的比例:
cluster0: 2.16%
cluster1: 2.33%
cluster2: 6.16%
cluster3: 5.70%

'SGS' 在不同 Cluster 的比例:
cluster0: 1.43%
cluster1: 1.39%
cluster2: 2.33%
cluster3: 1.77%

'認證' 在不同 Cluster 的比例:
cluster0: 1.83%
cluster1: 2.06%
cluster2: 3.26%
cluster3: 2.95%

### Cluster1 (舊版 cluster 3) 補充必須品型, 注重實用性、方便性、快速補貨、明確需求

'好用' 在不同 Cluster 的比例:
cluster0: 0.28%
cluster1: 0.37%
cluster2: 0.19%
cluster3: 0.20%

'限量' 在不同 Cluster 的比例:
cluster0: 0.47%
cluster1: 0.56%
cluster2: 0.28%
cluster3: 0.20%

### note: cluster 3 買日本比較多
'台灣' 在不同 Cluster 的比例:
cluster0: 3.29%
cluster1: 3.19%
cluster2: 3.08%
cluster3: 1.96%

'日本' 在不同 Cluster 的比例:
cluster0: 5.88%
cluster1: 6.01%
cluster2: 5.97%
cluster3: 7.47%

'韓國' 在不同 Cluster 的比例:
cluster0: 2.42%
cluster1: 2.58%
cluster2: 1.40%
cluster3: 0.20%