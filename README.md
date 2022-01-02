# 專案的目的和結果(推薦分數) 
## 目的: 
使用content-based建立推薦系統，使用structured和unstructured data
## 結果: 
* 與之前的rule-based比較數據增加10倍，但主要是因為之前rule建立的邏輯不完善
* 由於2018-09-01過後大多為新客戶，故需要加上rule-based提供推薦商品
* rule-based改為使用最熱門的商品即可大程度改善推薦分數
| random-based  | rule-based | content-based + rule-based_new |
| ------------- | ------------- | ------------- |
| 0.003389830508474576  | 0.013559322033898305  | 0.13389830508474576  |

# 工具、方法與原因
## 原因:
### Content-Based可以使用的variables分為structured和unstructured兩種:
* unstructured: title, description, feature
- 處理unstructured data的方法包含以下: 去除特殊字元、去除stopwords以及小寫
- EDA:分別觀察Token Frequency Distribution、Bigrams Frequency Distribution和Trigrams Frequency Distribution在移除stopwords的分布，其實資料中還是有不少的無意義的字眼，像是br br
- 原本想使用nltk.corpus import words解決無意義字眼問題，但evaluate結果較差
* structured: price, rank, brand, subcategory(extract from rank)
- 因時間不足以建立adjusted cosine similarity，暫時先將brand和subcategory作為unstructured的text一部分處理
### EDA資料觀察還包含以下
* testing data中的新客佔大多數，故加上rule-based彌補content-based無法提供推薦產品的部分
* Mimi的產品大多為美妝商品，且有季節流行性，故只抓取過去三個月資料作為訓練資料
* unstructured的feature資料不多，則不予考慮

## 方法步驟:
1. 將代表unstructured data的title, description以及structured data的brand, subcategory合成為一個新欄位unstructured
2. 對新的text欄位做去除特殊字元、去除stopwords以及小寫處理
3. TfidfVectorizer轉化為矩陣
4. 使用cosine_similarity計算商品之間feature的距離並排序
5. 若為新user或是過去三個月沒有消費的user，怎使用rule-based產生最暢銷商品，若為舊的user，則使用以上的content-based
