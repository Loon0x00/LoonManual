# LoonManual
Loon app manual

# 概述
Loon 是一个iOS网络代理工具，目前支持ss代理。
**IMPORT：Loon不提供具体的代理服务器地址，需自行搭建或者购买服务器。**

# 特性
* 自定义代理规则
* 简单的HTTP数据抓取
* 流量统计
* 支持IPV4/IPV6
* 支持ShadowSocks

# 配置文件
Loon自带一个默认的配置文件，默认配置文件自带一个Direct的无服务器节点，点击代理tab页面的default进入当前配置页面。

<div align=center><img width="310" height="552" src="https://github.com/Loon0x00/LoonManual/blob/master/Resource/Images/IMG_2088.PNG?raw=true"/></div>

当前配置页面显示了该配置下的所有代理节点，前面有绿色原点的表示当前应用的代理节点。

<div align=center><img width="310" height="552" src="https://github.com/Loon0x00/LoonManual/blob/master/Resource/Images/IMG_2089.PNG?raw=true"/></div>

点击当前配置文件条目进入全部配置文件列表，点击右上角的+可以添加配置文件，目前支持复制当前配置文件和复制默认配置文件，每个配置文件都可以进行单独的编辑。

## 具体配置和规则
具体配置包括配置代理服务器和配置匹配规则。

<div align=center><img width="310" height="552" src="https://github.com/Loon0x00/LoonManual/blob/master/Resource/Images/IMG_2091.PNG?raw=true"/></div>

### 配置代理服务器
Loon目前只支持Direct和ShadowSocket两种代理服务器，Direct表示所有流量不经过中间代理服务器，直接转发到目标服务器，ShadowSocket表示流量会去往支持ShadowSocket的代理服务器。
### 配置匹配规则

<div align=center><img width="310" height="552" src="https://github.com/Loon0x00/LoonManual/blob/master/Resource/Images/IMG_2092.PNG?raw=true"/></div>

Loon会根据匹配规则中的转发策略对每一个请求进行不同的处理，目前的匹配类型有以下几种：

1. **DOMAIN-SUFFIX 基于域名后缀**进行判断
2. **DOMAIN 基于域名**进行判断
3. **DOMAIN-KEYWORD 基于域名关键词**进行判断
4. **IP-CIDR 局域网匹配**
5. **FINAL 默认策略**
	
Loon包含三种策略：

1. Direct 流量直接转发到目标服务器
2. Proxy 流量转发到代理服务器
3. Reject 拒绝流量访问

匹配内容表示匹配的关键词、域名、局域网等
	
	#基于域名后缀判断是否匹配，匹配走远端代理服务器，进行远端dns解析
	DOMAIN-SUFFIX,twimg.com,PROXY,force-remote-dns
	#判断是否是局域网，是走代理，不需要进行dns解析
	IP-CIDR,91.108.56.0/22,PROXY,no-resolve

### 文本编辑配置
配置文本主要包括Proxy和Rule两部分：

<div align=center><img width="310" height="552" src="https://github.com/Loon0x00/LoonManual/blob/master/Resource/Images/IMG_2093.PNG?raw=true"/></div>

1、[Proxy]指定代理方法、服务器、密码

	[Proxy]
	🌍 Direct = Direct
	🇺🇸 US = ShadowSocks,45.56.67.78,443,rc4-md5,password
	
从左至右依次是名称、代理协议、代理服务器地址、加密方式、密码

2、[Rule]指定域名或IP地址对应的代理模式

	[Rule]
	#Type:DOMAIN-SUFFIX,DOMAIN,DOMAIN-KEYWORD,IP-CIDR
	#域名匹配规则:域名后缀匹配,全部域名匹配,域名关键字匹配,IP地址CIDR匹配
	
	#Strategy:DIRECT,Proxy,REJECT
	#代理策略:直接连接,走代理,拒绝连接
	
	#Options:force-remote-dns(Default:false),no-resolve
	#其他选项:强制远端DNS,不进行DNS解析
	
	DOMAIN-KEYWORD,google,PROXY,force-remote-dns
	DOMAIN-SUFFIX,twimg.com,PROXY,force-remote-dns
	DOMAIN-SUFFIX,google.cn,DIRECT
	IP-CIDR,91.108.56.0/22,PROXY,no-resolve
	
	FINAL,PROXY
	#FINAL表示默认的代理策略

一般情况下Loon自带的配置文件已经足够使用，如若需要添加代理规则可以在最近请求中查看请求host，然后根据host添加规则。

# HTTP数据包抓取
Loon目前仅支持基本的HTTP Header数据包抓取，后续body和HTTPS数据包的抓取正在开发中。
<div align=center><img width="310" height="552" src="https://github.com/Loon0x00/LoonManual/blob/master/Resource/Images/IMG_2094.PNG?raw=true"/></div>

# 常见问题
**Q**: 配置文件已经设置完成，vpn也开启为什么无法连接外网？

A: 查看你的配置的代理服务器ip是否正确；在手机设置中查看Loon的**WLAN与蜂窝移动网**是否勾上。

**Q**: 如何填写配置中的代理服务器？

A: Loon只是一个网络代理工具，不提供代理服务器，需要自行搭建服务器，具体的服务器地址和端口可咨询服务器提供商。

