## Auth 接口测试

测试 OAuth|OAuth2.0 授权接口


授权接口开发完毕，并且已经部署到如下地址

https://sample-driver.rokid.com/driver

现在我们就可以开始测试了。

### 测试 获取 OAuth 授权接口地址

OAuth 接口

```bash
POST /driver/command HTTP/1.1
Host: sample-driver.rokid.com
Content-Type: application/json
Cache-Control: no-cache
Postman-Token: 1d89f10f-b14d-e37e-fa9e-8deb3fce02e7

{
  "command": "OAuth",
  "params": {
    "callbackURL": "https://s.rokidcdn.com/path?u=1&v=2"
  }
}
```

期望返回结果

```
{
    "status": 0,
    "data": "http://sample-driver.rokid.com/oauth/authorize?callbackURL=https%3A%2F%2Fs.rokidcdn.com%2Fpath%3Fu%3D1%26v%3D2"
}
```

### 测试授权逻辑

拿到结果的， 在浏览器打开， url `http://sample-driver.rokid.com/oauth/authorize?callbackURL=https%3A%2F%2Fs.rokidcdn.com%2Fpath%3Fu%3D1%26v%3D2`

授权成功，会重定向到 `https://s.rokidcdn.com/path?u=1&v=2&userId=foo&userToken=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6ImZvbyIsImlhdCI6MTUxMzA1MDM0OSwiZXhwIjoxNTEzMDUxNTQ5fQ.YlKZmgPzNwZCWYrhMjbtU7Q8n39STr_tTRU1rRu2Ngk&expiresIn=1200&expiredTime=1513051549543`

检查项：

1. URL 里面包含 CallbackURL 所有信息， 路径，参数
2. OAuth 不同结果判断
  1. 如果是 OAuth2.0 需要 URL 返回 code 参数
  2. 如果 是 OAuth， 需要 URL 返回 userAuth 相关参数
3. [OAuth2.0] 通过 code 调用 OAuthGetToken command接口

### 『OAuth2.0』 检查通过 code 获取 userAuth

通过 code 调用 `OAuthGetToken` command接口

```bash
POST /driver/command HTTP/1.1
Host: sample-driver.rokid.com
Content-Type: application/json
Cache-Control: no-cache

{
  "command": "OAuthGetToken",
  "params": {
    "code": "xxxx"
  }
}
```

期望返回结果

```json
{
  "status": 0,
  "data":  {
    "userId": "xxx",
    "userToken": "yyyy",
    "expiredIn": 3600,
    "refreshToken": 3600
  }
}
```


### 检查 token 刷新

通过 code 调用 `OAuthGetToken` command接口

```bash
POST /driver/command HTTP/1.1
Host: sample-driver.rokid.com
Content-Type: application/json
Cache-Control: no-cache

{
  "command": "OAuthRefreshToken",
  "params": {
    "userId": "xxx",
    "userToken": "yyyy",
    "expiredIn": 3600,
    "refreshToken": 3600
  }
}
```

期望返回最新的 userAuth 信息

```json
{
  "status": 0,
  "data":  {
    "userId": "xxx",
    "userToken": "yyyy",
    "expiredIn": 3600,
    "refreshToken": 3600
  }
}
```
