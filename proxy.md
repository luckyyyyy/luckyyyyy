
# Perfma Proxy 录制器接口文档

录制器 BaseURL：https://localhost:10980/cgi-bin/

## 录制器版本信息
GET /info

## 下载证书
GET /cert

## 查看状态
GET /status

## 开始录制
POST /start/{clientId}

入参：其中`clientId`为选填，填写后返回指定的`clientId`而非随机值。
返回：返回一个`clientId`作为客户端标识符，5秒内不调用心跳则自动视为停止，下面接口会用到。

## 心跳
POST /heartbeat/{clientId}

入参：其中`clientId`为必填。
过滤：body部分可以传json，过滤格式如下，根据字段名过滤，多个匹配项使用[]。
返回：到当前时刻录制的信息，如果需要分页可以传`lastRowId`字段，如遇到pending可以传ids数组，里包含`lastRowId`，非特殊情况建议使用全量查询。
```json
{
 "lastRowId": 123,
 "ids": [123, 456],
 "filters": {
  "req": {
   "headers": {
    "host": ["www.baidu.com"]
   }
  }
 }
}
```

## 停止录制
POST /stop/{clientId}

入参：其中`clientId`为必填。
过滤：body部分可以传json，过滤格式如下，根据字段名过滤，多个匹配项使用[]。
```json
{
 "filters": {
  "req": {
   "headers": {
    "host": ["www.baidu.com"]
   }
  }
 }
}
```
