# data-course-sample
## 專案目的
以Amazon提供的美妝商品資料及評論資料建立適合美妝電商營運使用的商品推薦系統，改善電商平台之轉換率及提高客單價
## Milestone
* week 1: 實作rule-based的推薦系統
## rule-based方法跌代過程
### baseline: 將商品依照review數量由多至少排序當作熱銷商品排行榜
* Recall: 0.083
### method 2: 加入使用者在訓練期間的消費紀錄，若有消費紀錄則推薦歷史商品的also buy及also view，不足者以熱銷排行補足
* Recall: 0.083
### method 3: 考量到過久遠的交易紀錄可能沒什麼用，將訓練資料和交易紀錄的取用限定在距測試期1個月~1季之間
* 1個月(30天) Recall: 0.15763
* 2個月(60天) Recall: 0.15254
* 3個月(90天) Recall: 0.13389
### method 4: 將判斷使用者過去消費紀錄的部份移除，以比較訓練資料帶來的影響
* 1個月(30天) Recall: 0.15763
* 2個月(60天) Recall: 0.15254
* 3個月(90天) Recall: 0.13389
## 小結
可以發現目前的做法also buy及also view對於推薦的效果幾乎沒有影響，反而是距離越近的訓練資料效果越顯著
## package
* pandas
* json
* gzip
* datetime


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
