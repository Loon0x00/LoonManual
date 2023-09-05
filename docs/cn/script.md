# 脚本类型

## http-request
在获得一个http请求时触发，可配置的参数：
- `requires-body = true`代表是否获取http请求的body
- `binary-body-mode = true`代表获取http请求的body类型为Uint8Array
- `argument = "name=loon&version=2.1.0"`脚本中可以通过变量`$argument`获取，**为了能够准确解析到参数，请将参数用双引号包裹**

### 配置语法
```
http-request ^https?:\/\/(www.)?(example)\.com script-path=localscript.js,tag = requestScript,enable=true,requires-body = true
```
此类脚本中可以使用如下变量

- 所有[Script API](cn/script_api.md)
- `$request`: 一个http请求信息
    - `$request.url`: String类型，请求URL
    - `$request.method`: String类型，请求方法
    - `$request.headers`: js对象，请求头
    - `$request.body`: String或者Uint8Array类型，当`requires-body = true`时才有值，请求的body
- `$response`: undefined
- `$done()`方法参数说明：
    - `$done()`: 不传任何参数，表示放弃该请求，请求连接会直接断开
    - `$done({})`: 空js对象，请求继续，任何请求参数不会有任何变化
    - `$done({url:"https://new.example.com/",headers:{},node:"HK"})`: url参数替换原来的url，headers替换原来的request headers，node参数表示该请求后续用指定的策略组（也可以是节点名称）进行请求（build 534版本开始适用）
    - `$done({response:{
        status:200,
        headers:{},
        body:""
    }})`: 直接向该请求返回一个假的响应，body类型可以是String或者Uint8Array，**如果body的长度大于0，会自动计算headers中的content-length，content-encoding也会根据body类型自动生成**
    **注意** 当$done()参数的对象中的response不包含headers或者body时，表示使用原请求的headers和body，如果要清除原请求的body时，参数的`body=""`即可，清除原headers的话`headers={}`

## http-response
在获得一个http响应时触发，参数`requires-body = true`代表是否获取http响应的body
- `requires-body = true`代表是否获取http请求的body
- `binary-body-mode = true`代表获取http请求的body类型为Uint8Array
- `argument = "name=loon&version=2.1.0"`脚本中可以通过变量`$argument`获取，**为了能够准确解析到参数，请将参数用双引号包裹**

### 配置语法
```
http-response ^https?:\/\/(www.)?(example)\.com script-path=https://example.com/loon.js,timeout=10,requires-body = true,tag = responseScript,enable=true
```
此类脚本中可以使用如下变量
- 所有[Script API](cn/script_api.md)
- `$request`: http请求信息
    - `$request.url`: String类型，请求URL
    - `$request.method`: String类型，请求方法
    - `$request.headers`: js对象，请求头
    - `$request.body`: String或者Uint8Array类型，如果请求带有body，此参数才有值
- `$response`: http响应信息
    - `$response.status`: 响应状态
    - `$response.headers`: 响应头
    - `$response.body`: String或者Uint8Array类型，如果响应带有body，并且requires-body = true时此参数才有值
- `$done()`方法参数说明：
    - `$done()`: 不传任何参数，表示放弃该请求，请求连接会直接断开
    - `$done({})`: 空js对象，请求继续，任何请求参数不会有任何变化
    - `$done({
        status:200,
        headers:{},
        body:""
    })`: 直接向该请求返回一个假的响应，body类型可以是String或者Uint8Array，**如果body的长度大于0，会自动计算headers中的content-length，content-encoding也会根据body类型自动生成**
    **注意** 当$done()参数的对象中的request不包含headers或者body时，表示使用原请求的headers和body，如果要清除原请求的body时，参数的`body=""`即可，清除原headers的话`headers={}`

## cron
- 根据设定的cron表达式定时触发脚本
- "* * * * *" : 分 时 日 月 周 
- "* * * * * *" :秒 分 时 日 月 周

### 配置语法
```
cron "0 8 * * *" script-path=cron.js,tag = responseScript,enable=true
```
此类脚本中可以使用如下变量
- 所有[Script API](cn/script_api.md)

## network-changed
当网络环境发生变化时会调用此类型脚本，如果有多个这种类型的脚本，只会调用配置文件中的第一个
### 配置语法
```
network-changed script-path=https://raw.githubusercontent.com/Loon0x00/LoonExampleConfig/master/Script/netChanged.js, tag=changeModel,enable=true
```
此类脚本中可以使用如下变量
- 所有[Script API](cn/script_api.md)

## generic
以节点、策略组、规则等配置为参数的脚本，需要在app内部页面手动进行触发，不会主动触发
### 配置语法
```
generic script-path=https://raw.githubusercontent.com/Loon0x00/LoonExampleConfig/master/Script/generic_example.js,tag=GeoLocation,timeout=10,img-url=location.fill.viewfinder.system
```
此类脚本中可以使用如下变量
- 所有[Script API](cn/script_api.md)