# Pickel_PRD 規格書

[TOC]
## 1. 產品簡介及說明

Pickel 是一個選擇日期的線上小工具，讓大家可以利用這項工具簡單化挑選時間的過程。

## 2. 產品架構及流程

### 2.0 名詞定義  

發現會用到很多名詞，所以先來定義一下

#### 基本名詞
- 把一個投票活動，稱之為 event
- 發起投票的行為稱為 launch event，發起者稱為 launcher，中文叫做發起者
- 在這個 event 中選擇自己可以選擇的時間這個行為稱為 pick，選擇的人稱作 picker，中文叫做參與者
- laucher 本身也可以做為一位 picker

#### event 的基本要素
event 有一些基本要素：

- Range 活動時間範圍：最初，event 會提供 picker 的有一個基礎的範圍稱作 event range  

    像是我只開放 10/2 (一) 、 10/3 (二) 的下午可以讓 picker 們做選擇，這段時間就稱作為 range

    也有另外一個可能。如果我的活動是全天的，那我只開放說 10/2 ~ 10/9 可以讓 picker 做選擇。

- Duration/Limit 活動時長

    我的這個 event 總共要辦理多久，稱做為一個 Duration，而這個東西同時也是一個 Limit，Picker 選擇的 Period 不能小於 Limit，最後決定出來的確切時間長度為 Duration

    Duration/Limit 必須小於等於 Range

    Duration/Limit 有兩種選擇：全日 or 時間區段

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

- ready：已經設定好基本選項，但還沒開始投票
  
- picking：提供大家做選擇
  - 在這段期間不給設定 range 以及 duration
- endPicking：投票完成，但還沒有確定時間
  - 當下時間超過 picling end 
  - 在這段期間可以重新設定 duration

- determining：已經確定時間的活動

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

### 2.1 functional map
functional map 基本版
[functional map](https://coggle.it/diagram/X5pUT8xkJHH1rLV0/t/pickel/f697bb39a1f1d059b11c36cca421abcaa5f6784a8c4018a5ebefebe22b97e860)

### 2.2 IA
[IA](https://coggle.it/diagram/X5pzSYd4rgBvPujY/t/pickel/7eeb6aa5700359e602b4440e86ba18d7870a13f69ed9a99b37282a9d27d88884)

### 2.3 Flow chart
用戶方面的 flow Chart
https://miro.com/welcomeonboard/HE1ungDhs7GQTMv8zZu7l9ws7N5WSmyuCz2MTAw98HqNzWK6Fkb6V0Wg1a9Zvsjg



## 3. 產品設計原型

### 3.1 wireframe
https://miro.com/welcomeonboard/HE1ungDhs7GQTMv8zZu7l9ws7N5WSmyuCz2MTAw98HqNzWK6Fkb6V0Wg1a9Zvsjg

### 3.2 Prototype


