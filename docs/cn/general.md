# 其他配置
#### 该部分主要包含配置文件中 [general]模块下的参数解释

## bypass-tun
目前iOS设备上的流量有两种方式传递给Loon，分别是HTTP Proxy和TUN（可以简单理解为虚拟网卡），bypass-tun则和TUN有关，如果配置了该参数，那么所配置的这些IP段、域名就会不交给Loon来处理，系统直接处理
```
bypass-tun = 192.168.0.0/16,localhost,*.local
```
## skip-proxy
和上面类似，skip-proxy则和HTTP Proxy有关，如果配置了该参数，那么所配置的这些IP段、域名将不会转发到Loon，而是由系统处理
```
skip-proxy = 192.168.0.0/16
```
## dns-server
udp类的dns服务器，用,隔开多个服务器，system表示系统dns
```
dns-server = system,1.1.1.1
```
## doh-server
DNS over HTTPS服务器，用,隔开多个服务器
```
doh-server = https://doh.dns.apple.com/dns-query
```
## doq-server
DNS over QUIC服务器，用,隔开多个服务器，默认端口784
```
doh-server = quic://example.com, quic://example2.com
```
## doh3-server
DNS over HTTPS服务器，用,隔开多个服务器
```
doh3-server = h3://223.6.6.6/dns-query
```
## ipv6
是否允许IPV6的请求，开启后会进行DNS AAAA记录查询，并且优先使用IPV6的IP
```
ipv6 = true
```
## allow-wifi-access
是否开启局域网代理访问
```
allow-wifi-access = true
```
## wifi-access-http-port
开启局域网访问后的http代理端口
```
wifi-access-http-port = 8899
```
## wifi-access-socks5-port
开启局域网访问后的socks5代理端口
```
wifi-access-socks5-port = 8898
```
## proxy-test-url
测速所用的测试链接，如果策略组没有自定义测试链接就会使用这里配置的
```
proxy-test-url = http://cp.cloudflare.com/generate_204
```
## test-timeout
节点测速时的超时秒数
```
test-timeout = 5
```
## switch-node-after-failure-times
一个节点连接失败几次后会进行节点切换，默认3次
```
switch-node-after-failure-times = 2
```
## resource-parser
订阅资源解析器链接，推荐Peng大的sub-store订阅节点解析器
```
resource-parser = https://raw.githubusercontent.com/Peng-YM/Sub-Store/master/backend/dist/sub-store-parser.loon.min.js
```
## ssid-trigger
当切换到某一特定的WiFi下时改变Loon的流量模式，如"loon-wifi5g":DIRECT，表示在loon-wifi5g这个wifi网络下使用直连模式，"cellular":PROXY，表示在蜂窝网络下使用代理模式，"default":RULE，默认使用分流模式
```
ssid-trigger = "loon-wifi5g":DIRECT,"cellular":PROXY,"default":RULE
```
## real-ip
有些app会自己去请求DNS获取IP，这样导致有些域名类型的规则无法进行匹配，所以Loon是用了FakeIP来解决这个问题，原理是截取这些DNS请求，返回一个假的IP响应，然后在获取到这个假的IP的请求时将相关域名映射到请求中；但是有时候系统的一些域名会缓存这些假IP，导致关闭Loon后会用这个假的IP直接发起请求，这就会导致一些问题，针对这种情况可以配置real-ip来使这些域名返回真实的ip
```
real-ip = *.apple.com,*.icloud.com
```
## interface-mode
指定流量使用哪个网络接口进行转发，目前包含三种模式:
- Auto: 系统自动分配
- Cellular: 在WiFi和蜂窝数据都开启的情况下指定使用蜂窝网络
- Performace: 在WiFi和蜂窝数据都开启的情况下使用最优的网络接口
- Balance: 在WiFi和蜂窝数据都开启的情况下，均衡使用网络接口
```
interface-mode = Performace
```
## force-http-engine-hosts
有些app使用原始的tcp来进行http请求，这些流量就会走TUN，为了性能考虑，Loon默认只会解析80端口的这些http请求，如果一些请求的端口不是80，则可以在这里指定相关的域名或者端口，从而让Loon对这些http请求进行解析
```
// :8080，表示解析所有8080端口，0表示解析所有端口
// 通配符域名，解析所有端口下的相关域名
force-http-engine-hosts = *.baid.com,:8080
```
## disable-udp-ports
禁用udp协议的一些端口
```
disable-udp-ports = 443,80
```
## disable-stun
禁用stun是否禁用stun协议的udp数据，禁用后可以有效解决webrtc的ip泄露
```
disable-stun = true
```
## geoip-url
自定义geoip数据库的url