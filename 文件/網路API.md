# 網路API
所有網路 API 的網址開頭為 `https://catbud.net/api/` 或 `wss://catbud.net/api/`，有些需要令牌才能使用。想要獲得更多 API 端點，請在 Discord 上找 `@SYSTEM`。  
*測試服為 `https://catbud.net/api_test/` 或 `wss://catbud.net/api_test/`。*

## 定義
- `玩家`：可以輸入玩家的 UUID 或者名稱。
- `令牌`：從選單內創建的令牌，看起來像這樣：`6579abcf-7633-4654-a17f-aff077e02476`。
- `領地`：由數字組成的編號，不會小於 `0`。
- `身分`：領地身分組的 UUID。
- `密鑰`：[通知器](物品/通知器.md)、[遙控器](物品/遙控器.md)的秘密鑰匙。
- `貨幣`：由 `A` ~ `Z` 組成的字串，最多不超過 `4` 個字符。
- `價格`：由數字組成的數值，不會小於或等於 `0`。
- `維度`：從 `normal`（主世界）、`nether`（地獄）、`the_end`（終界） 中選擇一個。

`*` 表示可有可無。

## /range?env=`維度`&x=`X軸`&y=`Y軸`&z=`Z軸`&range=`範圍`
以 `X軸`、`Y軸`、`Z軸` 為中心點半徑 `範圍` 內的所有玩家清單，`範圍` 不能大於 `128`。
```json
[{"env":"normal","x":81.39646256082644,"y":118.99510369326832,"z":25.63120999311864,"yaw":-22.950226,"pitch":-0.7500416,"player":"63c09d08-a16d-4923-88fc-91df490c9e3b"}]
```

## /gateway?token=`令牌`
建立一個持久性 [WebSocket](https://zh.wikipedia.org/zh-tw/WebSocket) 連線。
- `init` 初始化。
- `heartbeat` 心跳（用於確定客戶端是否超時）。
- `wallet` 錢包事件。
- `payment` 申請付款事件。
- `own_join` 自身登入。
- `own_pos` 自身位置。
- `own_meta` 自身數據。
- `own_quit` 自身登出。
- `entity_add` 實體加入。
- `entity_pos` 實體位置。
- `entity_remove` 實體離開。
- `land_notify` 領地通知器（只有領地管理者才會接收事件）。

## /remote/`密鑰`/`能量`
`能量` 可以是 `true`（有信號）或 `false`（無信號），控制遙控器輸出的紅石信號。  
```json
{"env":"normal","x":79,"y":74,"z":28,"powered":true}
```

## /notify/`密鑰`
獲得通知器受到的紅石信號。
```json
{"env":"normal","x":77,"y":74,"z":28,"powered":false}
```

## /player/`玩家`/face
取得已登入過伺服器的玩家面部圖片。  
<https://catbud.net/api/player/xuancat/face>  
![xuancat](https://catbud.net/api/player/xuancat/face)

## /player/`玩家`/info
取得已登入過伺服器的玩家資料。  
<https://catbud.net/api/player/xuancat/info>  
```json
{"uuid":"63c09d08-a16d-4923-88fc-91df490c9e3b","name":"xuancat","locale":"zh_TW","discord":455305052416901130,"register":1703914504234,"update":1710316192929,"verified":true}
```

## /land/roles?token=`令牌`&land=`領地`
取得領地所有身分組名稱和UUID。
```json
{"87b9a7ba-58e2-45a1-9921-b60443de051d":"團長"}
```

## /land/members?token=`令牌`&land=`領地`
取得領地所有玩家授予的身分組。
```json
{"63c09d08-a16d-4923-88fc-91df490c9e3b":"87b9a7ba-58e2-45a1-9921-b60443de051d"}
```

## /land/define?token=`令牌`&land=`領地`&player=`玩家`&role=`*身分`
設置玩家在領地內的身分組，身分為 `null` 則撤銷該名玩家的身分組。
```json
{"63c09d08-a16d-4923-88fc-91df490c9e3b":"87b9a7ba-58e2-45a1-9921-b60443de051d"}
```

## /own/friends?token=`令牌`
取得自身好友清單。
```json
["07a1077a-c635-4348-a1a4-540b93bd8674","3991f9ee-2db8-46e1-bfe6-f8afe436f548","f37c3c16-b54c-48a4-98ba-2fe4f575f3ad"]
```

## /own/obstructs?token=`令牌`
取得自身封鎖清單。
```json
["98a74525-bedf-47f9-8dc7-c998ec5c02a1", "02fada27-2adf-4071-9144-75cd200af048","4f206d5b-82e2-4501-97f8-503cf075c2c5"]
```

## /payment/request?token=`令牌`&player=`玩家`&display=`說明`&prices=`價格清單`
*需要完成[認證](/選單/認證.md)才可使用此端點。*  
`說明` 必須被[百分號編碼](https://zh.wikipedia.org/zh-tw/百分号编码)，`價格清單` 使用 `貨幣:價格,貨幣:價格`，對指定的玩家要求付款，該玩家可從多種結帳方式內選擇一種。
```json
{"id":"13be49f5-772b-448e-9492-405b2c62274c","buyer":"b46966c8-148b-4b1e-a596-1b3f78d9831b","seller":"63c09d08-a16d-4923-88fc-91df490c9e3b","display":"測試商品啦！","currency":null,"price":null,"create":1707554240000,"update":1707554240000,"state":"wait"}
```
可以使用 `id` 作為 `結帳代號` 去查詢結帳狀態。

## /payment/result/`結帳代號`
查詢之前要求付款後現在的結帳狀態。
```json
{"id":"13be49f5-772b-448e-9492-405b2c62274c","buyer":"b46966c8-148b-4b1e-a596-1b3f78d9831b","seller":"63c09d08-a16d-4923-88fc-91df490c9e3b","display":"測試商品啦！","currency":"C","price":20,"create":1707554240000,"update":1707554721340,"state":"finish"}
```

## /currencies
取得伺服器有的全部貨幣代號。  
<https://catbud.net/api/currencies>  
```json
["A","C","T"]
```

## /transfer?token=`令牌`&player=`玩家`&currency=`貨幣`&amount=`金額`
轉帳指定金額給指定玩家。
```json
{"C":2449}
```

## /wallet?token=`令牌`&currency=`*貨幣`
取得自身錢包全部貨幣的剩餘金額。
```json
{"A":8,"C":2449,"T":262}
```

## /message?token=`令牌`&player=`玩家`&context=`內容`&author=`*作者`
`內容`、`作者` 必須被[百分號編碼](https://zh.wikipedia.org/zh-tw/百分号编码)，發送一則可以自定義 `作者` 的消息。
```json
{"player":"63c09d08-a16d-4923-88fc-91df490c9e3b"}
```
