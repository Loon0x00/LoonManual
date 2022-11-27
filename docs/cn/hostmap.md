# DNS映射

当需要对特定域名指定DNS服务或者固定IP时，可以使用此功能，目前支持以下几种模式：

- 域名映射域名
- 域名映射IP
- 域名指定查询DNS服务器
- 特定SSID环境下指定DNS查询的服务器

相关配置示例：
```
example.com = 192.168.1.20
example.com = example.com.cn
*.testflight.apple.com = server:8.8.4.4
// system表示系统DNS 服务器
*.apple.com = server:system
ssid:LOON's WIFI = server:system
ssid:LOON WIFI = server:https://example.com/dns-query
```