## DB schema
### Event

token = (rand(1,9) * pow(10, 9) + id).toString()
password 自行設置

{
    id pk
    name 名稱 varchar 
    description 描述 text
    launcher_id varchar
    picking_start 開始選擇時間 time
    picking_end 結束選擇時間 time
    duration 活動預計時長 int，如果 type 是 event_type 那麼就是 15 分鐘為單位，如果是 allday，那就是以 天 為單位
    detemine_time
    event_type [part, allday]
    token 
    event_suffix token_id varchar (rand(1,9) * pow(10, 9) + id).toString().base64 
    
    pick_suffix token_id 

    > 保持投票設定頁的隱私性，所以投票設定頁的 suffix 還有 時間選擇頁的 suffix 不一樣


    created_time
    updated_time
}



### range
{
    id 
    event_id
    range_start
    range_end
}

### token
{
    token_id
    token_id
}


### Pick

{
    id
    event_id FK to event.id
    name varchar
    created_time
    updated_time
    
}

### period 
{
    id int
    picker_id int
    start_time
    duration
    description
    priority [3, 2, 1]
}



### 回朔紀錄表


## API


投票發起（設定）網址 / 活動資訊網址
時間投票網址




getEvent
GET pickel.lauviah.io/event/event_suffix
GET pickel.lauviah.io/pick/pick

createEvent
POST pickel.lauviah.io 
=> {eventdata}
<= {event pickel.lauviah.io/eventtoken}

updateEvent
PATCH pickel.lauviah.io
=> {eventdata}
<= {eventData}

createPick


### API 格式

{
    ok: [1 , 0],
    message : '' 只有錯誤才會有 message 
    data {
        裡面依照需要的東西給資料
    }


}