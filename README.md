# 專案的目的和結果(推薦分數) 
## 目的: 
使用rule-based建立推薦系統，增加銷售率和推銷長尾商品
## 結果: 
與random-based比較數據增加了4.5倍，但rule建立的邏輯不完善，目前直覺認為使用 basket analysis 再配合一些rule可能效果會更好，因為時間不足，不然會想嘗試 basket analysis 搭配rule
| random-based  | rule-based |
| ------------- | ------------- |
| 0.003389830508474576  | 0.013559322033898305  |

# 工具、方法與原因
## 原因:
 一開始直覺想使用basket analysis，但題目的主題為rule-based，所以盡可能的dig into data，從中找到一些rule，觀察且考慮的有以下
i. 以下參數結合為一個product_score，做為評估是否為使用者browse以後容易下單的商品或是對公司的營收有助益  
    a. 商品在網頁上的資訊是否夠明確，有的話會增加購物者下單意願，例如是否有圖片或是敘述等  
         (title, feature, description. image, imageHighRes, tech1以上有資訊則+1，無則為0)  
    b. 如果商品得到製造商的discount，也視為對增加客單價的正向指標 (discountbyManufacturer有責+1)  
    c.  若商品附帶battery，則為1，無則0  
    d.  從rank拿到商品在分類中的排名，取log後作為分數使用  
ii. 品牌忠誠度: 評估玩家對品牌的忠誠度  
avg(count of purchase history per brand per user) per product  
iii. 使用者評價: overall  
avg(overall) per product  
iv.  product_popularity 可藉由商品熱銷度反推長尾商品  
count of purchase history per product  

## 方法步驟:
1. 確認使用者過去是否有品牌忠誠，若有則從使用者愛用品牌中列出相對應的product list，若無則random從挑出五個較熱門品牌列出相對應的product list  
2.  top25_by_productScore: 從product list中選product score前25高的商品  
3.  top15_by_avgReview : 從product score前25高的商品中挑出前15名avg_review較高的商品  
4. 最後再從15個商品中挑出最熱門的五個和最不暢銷的五個商品  
