# 網路API
所有的網路API的網址開頭為 `https://catbud.net/api/`，有些需要令牌才能使用。

## 規則
- `玩家`：可以輸入玩家的 UUID 或者名稱。
- `令牌`：從選單內創建的令牌，看起來像這樣：`6579abcf-7633-4654-a17f-aff077e02476`。
- `領地`：由數字組成的編號，不會小於 `0`。
- `身分`：領地身分組的 UUID。
- `密鑰`：[通知器](物品/通知器.md)、[遙控器](物品/遙控器.md)的秘密鑰匙。

`*` 表示可有可無。

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

## /player/`玩家`/face
取得已登入過伺服器的玩家面部圖片。  
![xuancat](https://catbud.net/api/player/xuancat/face)

## /player/`玩家`/info
取得已登入過伺服器的玩家資料。
```json
{"uuid":"63c09d08-a16d-4923-88fc-91df490c9e3b","name":"xuancat","locale":"zh_TW","discord":455305052416901130,"register":1703914504234,"update":1708008368617,"skin":"ewogICJ0aW1lc3RhbXAiIDogMTcwODAwODA4NTExNSwKICAicHJvZmlsZUlkIiA6ICI2M2MwOWQwOGExNmQ0OTIzODhmYzkxZGY0OTBjOWUzYiIsCiAgInByb2ZpbGVOYW1lIiA6ICJ4dWFuY2F0IiwKICAic2lnbmF0dXJlUmVxdWlyZWQiIDogdHJ1ZSwKICAidGV4dHVyZXMiIDogewogICAgIlNLSU4iIDogewogICAgICAidXJsIiA6ICJodHRwOi8vdGV4dHVyZXMubWluZWNyYWZ0Lm5ldC90ZXh0dXJlLzFkN2FlYzgzNmI5OGM1ZGRmMTNmZDFjM2QwMmJmMGFkYWUxYzIyODY5MzViNGJjNmNiNGVkODc1ZGVkOWY2ZjgiCiAgICB9CiAgfQp9"}
```

## /land/roles?token=`令牌`&land=`領地編號`
取得領地所有身分組名稱和UUID。
```json
{"87b9a7ba-58e2-45a1-9921-b60443de051d":"團長"}
```

## /land/members?token=`令牌`&land=`領地編號`
取得領地所有玩家授予的身分組。
```json
{"63c09d08-a16d-4923-88fc-91df490c9e3b":"87b9a7ba-58e2-45a1-9921-b60443de051d"}
```

## /land/define?token=`令牌`&land=`領地編號`&player=`玩家`&role=`*身分`
設置玩家在領地內的身分組，身分為 `null` 則撤銷該名玩家的身分組。
```json
{"63c09d08-a16d-4923-88fc-91df490c9e3b":"87b9a7ba-58e2-45a1-9921-b60443de051d"}
```
