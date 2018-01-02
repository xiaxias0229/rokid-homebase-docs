# Webhook

## 触发 Webhook

创建一个 HTTP POST 请求至

```
https://homebase.rokid.com/trigger/with/{your_very_awesome_token}
```

与一个可选的 JSON 请求 Body，如：

```json
{
  "type": "tts",
  "devices": {
    "sn": "a_very_random_serial_number_of_rokid"
  },
  "data": {
    "text": "Vive l'amour"
  }
}
```

你可以在命令行使用 `curl` 来尝试：

```
curl -X "POST" "https://homebase.rokid.com/trigger/with/{your_very_awesome_token}" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "type": "tts",
  "devices": {
    "sn": "a_very_random_serial_number_of_rokid"
  },
  "data": {
    "text": "Vive l'amour"
  }
}'
```

## 触发 Body

```yaml
---
type: object
properties:
  type:
    type: string
    enum:
      - tts
      - audio
  devices:
    type: object
    properties:
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
  data:
    type: object
```

### Data of Type: `tts`

```yaml
---
type: object
properties:
  text:
    type: string
    description: 播报内容
```

### Data of Type: `audio`

```yaml
---
type: object
properties:
  url:
    type: string
    description: 音频地址
    format: uri
    pattern: ^https?://
```

### 筛选设备

`devices` 属性的 `sn`，`roomName`，`tag` 和 `isAll` 共同筛选目标若琪设备，我们设一个在厨房的若琪 SN 为 `a_very_random_serial_number_of_rokid`，并且有 `拿破仑`，`雪球` 两个标签，则我们可以用以下条件筛选：

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
