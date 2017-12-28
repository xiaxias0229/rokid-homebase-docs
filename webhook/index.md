# Webhook

## 触发 Webhook

创建一个 HTTP POST 请求至

```
https://homebase.rokid.com/trigger/with/{your_very_awesome_token}
```

与一个可选的 JSON 请求 Body，如：

```json
{
  "tts": {
    "text": "Vive l'amour",
    "sn": "a_very_random_serial_number_of_rokid",
  }
}
```

你可以在命令行使用 `curl` 来尝试：

```
curl -X "POST" "https://homebase.rokid.com/trigger/with/{your_very_awesome_token}" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "tts": {
    "text": "Vive l\'amour",
    "sn": "a_very_random_serial_number_of_rokid",
  }
}'
```

## 触发 Body

```yaml
---
type: object
properties:
  tts:
    type: object
    properties:
      text:
        type: string
        description: 播报内容
      sn:
        type: string
        description: 若琪序列号
      roomName:
        type: string
        description: 若琪所处的房间
      tag:
        type: string
        description: 设备标签
      isAll:
        type: boolean
        default: false
        description: 选择所有设备
  audio:
    type: object
    properties:
      url:
        type: string
        description: 音频地址
        format: uri
        pattern: ^https?://
      sn:
        type: string
        description: 筛选序列号为指定值的设备
      roomName:
        type: string
        description: 筛选在房间名为指定值的房间中的设备
      tag:
        type: string
        description: 筛选标签为指定值的设备
      isAll:
        type: boolean
        default: false
        description: 选择所有设备
```

属性 `sn`，`roomName`，`tag` 和 `isAll` 共同筛选目标若琪，设一个在厨房的若琪 SN 为 `a_very_random_serial_number_of_rokid`，并且有 `拿破仑`，`雪球` 两个标签，则我们可以用以下条件筛选：

```
{
  "sn": "a_very_random_serial_number_of_rokid"
}
```

```
{
  "sn": "a_very_random_serial_number_of_rokid",
  "roomName": "厨房"
}
```

```
{
  "tag": "雪球"
  "roomName": "厨房"
}
```

```
{
  "tag": "拿破仑"
}
```
