# LoonManual
Loon app manual
Last update: 2019-11-04
Loon version: 1.2.3(80)

# 概述
- **[下载地址](https://itunes.apple.com/in/app/id1373567447)**
- **[配置文档](https://github.com/Loon0x00/LoonExampleConfig)**
- **[Twitter](https://twitter.com/loon0x00)**

Loon 是一个iOS网络代理工具，目前支持ss,ssr代理。
**IMPORTANT：Loon不提供具体的代理服务器地址，需自行搭建或者购买服务器。**

# 概念
### 节点（Node）
一个节点代表一个代理服务器，代理服务器可以实现流量的中转。
### 订阅节点（Subscription Node）
订阅节点是许多节点的集合，是一个配置在远端的节点集合，Loon通过url下载下来后解析成一个节点列表。
### 策略组（Policy Group）
一个策略组分为两部部分：策略类型，子策略。策略组会根据策略类型选出流量最终使用的节点
Loon目前支持三种策略：
1. **select** 根据用户UI选择使用哪个节点
2. **url-test** 默认每隔600s向配置的url发出http请求，选择最快返回数据的节点（注：url-test不支持策略组为其子策略）
3. **ssid**，根据配置的ssid名字，在wifi切换时自动切换节点，也可以配置默认节点和在蜂窝数据下使用的节点

一个策略组可以包含任何单个节点，订阅节点，策略组（url-test不除外），内置策略（DIRECT，REJECT）
### 规则（Rule）
规则决定了一个请求所使用的的策略。
规则分为三部分：规则类型，匹配关键字，策略名称
例如：`DOMAIN-SUFFIX,twimg.com,PROXY`
##### 规则类型
目前Loon支持6种匹配类型
1. **DOMAIN-SUFFIX 基于域名后缀**进行判断
2. **DOMAIN 基于域名**进行判断
3. **DOMAIN-KEYWORD 基于域名关键词**进行判断
4. **IP-CIDR 局域网匹配**
5. **FINAL 默认策略**
6. **User-Agent** 根据Http的user-agent值来进行匹配，支持带有*，？的通配符匹配

特殊规则类型 **Final**
表示如果没有匹配的规则，默认使用的策略
##### 规则策略名称
可以使用节点名称、订阅节点名称以及策略组名称

### URL Rewrite
在收到http请求时，Loon会使用请求的url去寻找匹配的url rewrite，一个URL Rewrite会根据匹配的规则替换或者修改HTTP请求中的url，或者替换请求响应体。
一个URL Rewrite分为三部分：正则表达式，替换内容，rewrite类型
例如：`https?://(www.)?g.cn https://www.google.com 302`
目前Loon支持4种类型URL Rewrite 类型：
1. **header** 请求头中匹配的host字段将会被替换内容所替换
2. **reject** 直接拒绝请求
3. **302** 返回302响应
4. **307** 返回307响应

### DNS
通过自定义DNS来快速、正确的获取Domain的IP。Loon 会根据你设置的DNS并发请求，使用响应最快的结果，并且使用LRU缓存最近的100个结果。

### MITM （Man in the Middle Attack）
中间人攻击方式解密https的请求。Loon会根据配置的hostname和信任的CA证书解密相应的https请求和响应，解密后可以配合Rule和URL Rewrite进行分流。

# 使用方式
### 添加单个节点
Loon目前支持三种方式添加单个节点：手动输入配置、粘贴uri、扫描二维码。
进入Profile->Single Node-> +  可进行相关节点配置
### 添加订阅节点
目前支持ssip02格式的ss uri，ssr官方订阅格式的节点订阅，由于某些订阅节点被墙，需要翻墙后进行下载，目前Loon会过滤掉port小于3的节点作为订阅机场的信息展示节点。
添加订阅的UI操作：Profile->Subscription Node-> +
### 添加策略组
**当添加完节点、订阅节点之后需要配置策略组才可以正常使用**，进入Profile->Policy Group-> + 进入添加Group页面后根据需要进行新建策略组，相关选项请参考上文中的策略组的概念。
### 使用策略组
在App 首页有一个Strategy模块，点击进入后会展示配置的策略组，可以选择select类型策略组所使用的的节点，也可以展开策略组进行Delay延迟测试。

# 常见问题
**Q**: 配置文件已经设置完成，vpn也开启为什么无法连接外网？

A: 查看你的配置的代理服务器ip是否正确；在手机设置中查看Loon的**WLAN与蜂窝移动网**是否勾上。

**Q**:添加订阅后无法看见订阅节点，选择节点
A:  添加完订阅并下载节点成功后会进入到节点列表页，此处无法选择节点，需要将订阅节点组合策略组使用

**Q**: 如何填写配置中的代理服务器？

A: Loon只是一个网络代理工具，不提供代理服务器，需要自行搭建服务器，具体的服务器地址和端口可咨询服务器提供商。

