## DB schema
### Event

token = (rand(1,9) * pow(10, 9) + id).toString()
password 自行設置

{
    id pk
    event_state [setting, picking],
    eventname 名稱 varchar 
    description 描述 text
    launcher_id 發起人 int FK launcher.id
    picking_start 開始選擇時間 time
    picking_end 結束選擇時間 time
    duration 活動預計時長 int，如果 type 是 event_type 那麼就是 15 分鐘為單位，如果是 allday，那就是以 天 為單位
    detemine_time
    event_type [part, allday]
    event_range_start 投票開始時間
    event_range_end 投票結束時間
    password 
    event_suffix token_id varchar (rand(1,9) * pow(10, 9) + id).toString().base64 
    pick_suffix token_id 
    一個 event 生成一個 JWT 的 token，防止有人進入自己的投票，然後向其他人的 event 發出 update 的 request 或者是 pick

    created_time
    updated_time
}

### token
{
    token_id
    token_id
}


### Launcher 
{
    id
    name varchar
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
    end_time
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