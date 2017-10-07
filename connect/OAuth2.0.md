## OAuth 2.0 需要实现 3 个接口

### 接口1. `OAuth`

输入参数
 * command {String} 方法名
 * params
 * params.callbackURL {String} 授权回调 URL

输出

* status {Number} 0 表示成功
* data {String} 授权跳转 URL


Exsample
```
{
  "command": "OAuth",
  "params": {
    "callbackURL": "http://s.rokidcdn.com/xxxxxx"
  }
}
```
返回结果，JSON格式

```
{
  "status": 0,
  "data": "http://my.driver.com/xxx?callbackurl=http%3A%2F%2Fs.rokidcdn.com%2Fxxxxxx"
}
```


### 接口2: OAuthGetToken

输入

* command {String} 方法名
* params.code {String}

输出

* - status {Number} 接口状态
* - data {Object}
* - data.userId 授权用户ID
* - data.userToken 授权token
* - data.refreshToken 刷新用的token
* - data.expiredTime {Number} 时间戳，单位秒
* - data.ext1 {String} 扩展字段
* - data.ext2 {String} 扩展字段
* - data.ext3 {String} 扩展字段
* - data.ext4 {String} 扩展字段
* - data.ext5 {String} 扩展字段

Example
```
{
  "command": "OAuth",
  "params": {
    "code": "XXXXXXXXXX"
  }
}
```
返回结果，JSON格式

- data { JSON }

```
{
  "status": 0,
  "data": {
    "userId": "XXXXXXXXXX",
    "userToken": "XXXXXXXXXX",
    "refreshToken": "XXXXXXXXXX",
    "expiredTime: 1503393264
  }
}
```


### 接口3: `OAuthRefresh`

输入 userAuth

* params.code {String} 授权 code
* data {Object}
* userId 授权用户ID
* userToken 授权token
* refreshToken 刷新用的token
* expiredTime {Number} 时间戳，单位秒
* ext1 {String} 扩展字段
* ext2 {String} 扩展字段
* ext3 {String} 扩展字段
* ext4 {String} 扩展字段
* ext5 {String} 扩展字段

输出 userAuth

* - status {Number} 接口状态
* - data {Object}
* - data.userId 授权用户ID
* - data.userToken 授权token
* - data.refreshToken 刷新用的token
* - data.expiredTime {Number} 时间戳，单位秒
* - data.ext1 {String} 扩展字段
* - data.ext2 {String} 扩展字段
* - data.ext3 {String} 扩展字段
* - data.ext4 {String} 扩展字段
* - data.ext5 {String} 扩展字段

传入参数

```
{
  "command": "OAuthRefresh",
  "params": {
    "userId": "XXXXXXXXXX",
    "userToken": "XXXXXXXXXX",
    "refreshToken": "XXXXXXXXXX"
  }
}
```
返回结果，JSON格式

- data { JSON }

```
{
  "status": 0,
  "data": {
    "userId": "XXXXXXXXXX",
    "userToken": "XXXXXXXXXX",
    "refreshToken": "XXXXXXXXXX",
    "expiredTime: 1503393264
  }
}
```