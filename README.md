# LoonManual
Loon app manual

# 概述
Loon 是一个iOS网络代理工具，目前支持ss代理。

#特性
* 自定义代理规则
* 简单的HTTP数据抓取
* 流量统计
* 支持IPV4/IPV6
* 支持ShadowSocks

#配置文件
Loon自带一个默认的配置文件，配置文件主要包括两个部分：
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

Loon支持多个配置文件管理，提供了可视化和文本编辑两种配置文件的管理。

一般情况下Loon自带的配置文件已经足够使用，如若需要添加代理规则可以在最近请求中查看请求host，然后根据host添加规则。

#HTTP数据包抓取
Loon目前仅支持基本的HTTP Header数据包抓取，后续body和HTTPS数据包的抓取正在开发中。


