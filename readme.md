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

---

# 2. 不同群的商品關鍵字
