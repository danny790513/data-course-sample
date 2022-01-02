# data-course-sample
## 專案目的
以Amazon提供的美妝商品資料及評論資料建立適合美妝電商營運使用的商品推薦系統，改善電商平台之轉換率及提高客單價
## Milestone
* week 1: 實作rule-based的推薦系統
* week 2: 針對有消費紀錄的使用者加入content-based的推薦方式
---
## week1迭代
>## rule-based (baseline):  
>將商品依照review數量由多至少排序當作熱銷商品排行榜
>* Recall: 0.083

>## rule-based method 2:  
>加入使用者在訓練期間的消費紀錄，若有消費紀錄則推薦歷史商品的also buy及also view，不足者以熱銷排行補足
>* Recall: 0.083

>## rule-based method 3:  
>考量到過久遠的交易紀錄可能沒什麼用，將訓練資料和交易紀錄的取用限定在距測試期1個月~1季之間
>|30天|60天|90天|
>|---|---|---|
>|0.15763|0.15254|0.13389|

>## rule-based method 4:  
>將判斷使用者過去消費紀錄的部份移除，以比較訓練資料帶來的影響
>|30天|60天|90天|
>|---|---|---|
>|0.15763|0.15254|0.13389|

## week1小結
可以發現目前的做法also buy及also view對於推薦的效果幾乎沒有影響，反而是距離越近的訓練資料效果越顯著

---
## week2迭代
>## content-based + rule-based:  
>將商品間cosine similarity > 0.5的商品定義為相似商品，若使用者曾有消費紀錄，則推薦歷史購買商品的相似商品，不足者以熱銷排行補足(rule-based method 3)
>|30天|60天|90天|
>|---|---|---|
>|0.15593|0.15085|0.13389|

## week2小結
本週嘗試性的加入基於內容的推薦方式，並沒有取到預期中大幅提升Recall的效果，原因可能是因為測試資料中584人中僅有38人(約6%)用戶曾有過消費紀錄，且這些消費紀錄亦未能提供品質較好的訊息，因此反而因為基於內容的推薦方式占用至少1個商品名額(曾購買過的商品cosine similarity=1必定會被推薦)，導致Recall稍微降低了一點

---
## package
* pandas
* json
* gzip
* datetime
---
## 其他嘗試
### 從metadata中推薦rank最高的商品
* Recall: 0.00508
### 從metadata中推薦price最便宜的商品
* Recall: 0.0
不work的原因: 最便宜的商品可能屬於某些配件或是特規商品，直接推薦效果不好
### training_data中推薦平均overall最高的商品
* Recall: 0.0 
* 不work的原因: 評論筆數可能過少，應針對評論數設置最低門檻
### training_data中推薦平均overall最高且高於n篇評論的商品
* n=100 Recall: 0.003389
* n=500 Recall: 0.0
* n=1000 Recall: 0.0
* n=1500 Recall: 0.0
* n=2000 Recall: 0.08305(剛好是最熱銷的top10)
* 不work的原因: 這種篩選方式有點不可靠，比較適合當作其他機制的輔助濾網
