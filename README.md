# 專案的目的和結果(Content-Based) 
## 目的: 
分別使用cf-user, cf-item and cf-surprise建立推薦系統，了解資料轉置以及計算使用者與商品距離和使用surprise套件
## 結果: 
* cf與之前的rule-based比較效果較差，主要還是得使用rule-based彌補cf
* 發現在surprise的training data時間控制是有影響的，過往rule-based發現商品的季節性多使用過去三個月資料，但cf-surprise的訓練資料在一年看起來結果最好
* rule-based使用最熱門的商品可大程度改善推薦分數

| random-based  | rule-based | cf-user | cf-item | cf-surprise |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| 0.003389830508474576  | 0.013559322033898305  | 0.00  | 0.00  | 0.001694915254237288  |

# 工具、方法與原因
## 原因:
### cf-user, cf-item, cf-surprise可以使用的variables主要為ratings
* ratings: reviewerID, asin, overall
* 在這次實作中只盡量讀懂資料轉置和整體流程，如果有時間會針對時間和overall做一些EDA以及k值的實驗

## 方法步驟:
### cf-user, cf-item
1. 按照課程流程將資料過濾並轉置
2. 使用轉置矩陣展開計算相似度
3. 計算 user similarity matrix
### cf-surprise
1. 留下最新評分的資料，將重複的過濾
2. 將資料轉成user / itme / rating 的順序
3. 設定KNN的套件，有做以下兩個嘗試
* 使用3個月的資料和1年的資料分別做實驗，1的的資料效果較好，3個月則趨近於0
* 調整min-k的數值，看起來該數值變化對結果影響不明顯
