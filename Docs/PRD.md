# Pickel_PRD 規格書

[TOC]

## 1. 產品簡介

Pickel 是一個選擇日期的線上小工具，讓大家可以利用這項工具簡單化挑選時間的過程。

## 產品故事

Pickel 可以解決挑選活動時間麻煩的問題。

舉個例子，小明以及他的快樂夥伴們開始奮發向上，想要辦一場讀書會，希望時間在 12/5(一) ～ 12/7(三) 下午 5:00 到 晚上 10:00 之間，讀書會的時間預計為 2hr。於是小明就可以到 Pickel 中發起一個 Event，並設定：

Range: 12/7(一) ～ 12/9(三) 17:00 到 22:00 之間
Duration: 2hr
Picking time: 11/22(日) ~ 11/26(四)
Event name: 糞發項上讀書會
Event launcher: Ming

當它設定活動好之後，可以讓將這個活動的連結發給他的快樂夥伴們，讓每個人選擇自己哪幾個時間比較方便。他的快樂夥伴們有 小美、小華，小明發起了 Event 之後，點擊 start picking 之後，會生成一個 url，這個連結可以發給活動的參加者，讓他們選擇自己哪些時間比較方便。

像是小美收到了這個連結並點擊，首先輸入自己的名稱，再來就可以選擇自己方便的時間，例如小美的容許時間是：

12/7 17:00 ~ 21:00 （最方便）
12/8 20:00 ~ 22:00 （如果可以盡量不要選擇這個時間）
12/9 17:00 ~ 18:30 （還 OK）

除了選擇容許時間，大家也可以在自己的時間後面加上附註，讓小明知道自己的時間狀況

雖然 12/7 17:00 ~ 18:00 小美可以活動的時間比預計的時間短，但依然可以選擇，可能大家並沒有足夠的容許時間，於是只好縮短活動時間爭取大家到場的機會。

當 11/26 大家選擇完自己的容許時間後，小明能夠知道大家的選擇，它可以直接檢視整個 Range，哪個時段最多人 OK，或者是詳細的觀看每個人的時段。

最後小明決定能夠活動的確切時間（和之前的活動長度不一定相同），並且生成一個網址，可以發給自己小夥伴們，讓他們知道確定的活動時間、內容。

## 2. 產品開發排程
1. 準備工作
   1. 決定前後端架構
   2. 開好前後端 API & 格式
   3. 稍微決定要用的技術 or 非關聯式資料庫（應該是關聯吧）=> 然後開好 DB 的 schema
2. 
3. 決定要用的技術
4. 熟悉要用的技術
5. 準備好開發的環境
  a. 開發環境
  b. 部署環境
8. 開發後端 API
9. 開發前端
   1.  

依照 PRD 開發前端


因為自己是一人團隊，而且時間稍微趕一點，所以一次 sprint 以一個禮拜為基準。

## 3. 產品架構及流程

### 3.0 名詞定義

發現會用到很多名詞，所以先來定義一下

#### 基本名詞

- 把一個投票活動，稱之為 event
- 發起投票的行為稱為 launch event，發起者稱為 launcher，中文叫做發起者
- 在這個 event 中選擇自己可以選擇的時間這個行為稱為 pick，選擇的人稱作 picker，中文叫做參與者
- laucher 本身也可以做為一位 picker
- 發起活動者，設定活動的網址稱作 event Page，中文為：投票設定頁
- pick 的頁面稱作 pick Page，中文：時間選擇頁

#### event 的基本要素

event 有一些基本要素：

- Range 活動時間範圍：最初，event 會提供 picker 的有一個基礎的範圍稱作 event range  
   Range 有兩種

  1. 時段：最小單位為 15 分鐘，最大時間為 23 hr 45 min (不可超過一天)，使用在非全天的活動
  2. 全日：最小單位一天，最大天數七天，使用在活動時間大於等於一天的活動

  像是我只開放 10/2 (一) 、 10/3 (二) 的下午可以讓 picker 們做選擇，這段時間就稱作為 range

  也有另外一個可能。如果我的活動是全天的，那我只開放說 10/2 ~ 10/9 可以讓 picker 做選擇。

- Duration/Limit 活動時長

  我的這個 event 總共要辦理多久，稱做為一個 Duration，而這個東西同時也是一個 Limit，Picker 選擇的 Period 不能小於 Limit，最後決定出來的確切時間長度為 Duration

  Duration/Limit 必須小於等於 Range

- determine time 最終活動時間：最後確定的時間
- Event name ：這個 Event 的名稱
- Event description：這個 event 的一些描述，附註
- Event launcher：event 的發起人
- Picking duration：投票期限，允許在哪段期間可以做 pick  
  picking Duration 不能晚於 Range
- picking Start 投票開始時間
  picking start 不能晚於現在時間
  格式為 YYYY/MM/DD hh:mm
  mm === [00, 15, 30, 45]

- picking end 投票結束時間
  picking end 不能晚於現在時間
  格式為 YYYY/MM/DD hh:mm
  mm === [00, 15, 30, 45]

一個 Event 有幾種狀態：

- prePicking：已經設定好基本選項，但還沒開始投票
  - 在這段期間，可以重新設定任何資訊，但是設定完需要點擊更新活動資訊
  - 更新後，可以點擊 提早開始活動，這會直接更新 pickstart 的時間
- picking：提供大家做選擇
  - 在這段期間只能更改 description，設定完也要點擊更新活動資訊，
  - 可以點擊暫停投票
    1. pickStart 改成 now
    2. 重新進入 prepicking
    4. 之前的投票保留
  - 可以點擊重新投票，重新進入 prepicking，然後之前的投票刪除
    1. pickStart 改成 now
    2. 重新進入 prepicking
    4. 之前的投票刪除
  - 可以點擊終止投票，
    1. pickEnd 改成 now
  - 可以看投票的狀況
- postPicking：投票完成，但還沒有確定時間

  - 當下時間超過 picling end
  - 在這段期間可以重新設定 duration, name, launcher, description
  - 可以點擊重新投票，重新設定 pickStart 變成 now，pickEnd = now + 1 day, 投票全部刪除
  - 可以設定 determined
  - 可以看投票的狀況
- determined：已經確定時間的活動
  - 全部都不能改動，只能看投票的狀況

#### Pick 的基本要素

- Picker Name 個人名稱：投票要要給個名子阿
- Period 個人時段：大家選擇的一段時間，稱作為 period
  我是 Picker，我在 10/2 (一) 的下午 2:00 ~ 3:00，以及 5:00 ~ 6:00 這段段時間 OK，所以我就有兩個 period。 Period 有幾項規定
  - Period 之間不可重複
  - Period 不能大於 range
    但是 Period 可以
  - 小於 Limit/Duration
    每個 Period 也有選項
  - Priority：

### 3.1 functional map

functional map 基本版
[functional map](https://coggle.it/diagram/X5pUT8xkJHH1rLV0/t/pickel/f697bb39a1f1d059b11c36cca421abcaa5f6784a8c4018a5ebefebe22b97e860)

### 3.2 Infomatino Archetecture

[IA](https://coggle.it/diagram/X5pzSYd4rgBvPujY/t/pickel/7eeb6aa5700359e602b4440e86ba18d7870a13f69ed9a99b37282a9d27d88884)

### 3.3 Flow chart

用戶方面的 flow Chart
https://miro.com/welcomeonboard/HE1ungDhs7GQTMv8zZu7l9ws7N5WSmyuCz2MTAw98HqNzWK6Fkb6V0Wg1a9Zvsjg

## 4. 產品設計原型

### 4.1 wireframe

https://miro.com/welcomeonboard/HE1ungDhs7GQTMv8zZu7l9ws7N5WSmyuCz2MTAw98HqNzWK6Fkb6V0Wg1a9Zvsjg

### 4.2 Prototype
