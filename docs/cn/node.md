# 节点（代理服务器）

一个节点表示一个代理服务器，你可以手动添加单个节点，也可以通过链接下载订阅节点。**Loon 本身不提供任何的节点**

## 订阅节点

订阅节点是你的服务提供商提供的节点的集合，Loon只负责下载、解析这些节点，在Loon App中是无法修改这些节点的，如需修改请联系您的订阅提供商

### 关于订阅节点的流量明细
目前Loon会读取订阅响应header中的Subscription-Userinfo信息，具体格式如下：
```
Subscription-Userinfo:upload=1111;download=111;total=123456;expire=1614527045
```

## 代理服务协议
代理服务协议指的是在进行网络传输的过程中客户端和服务端需要遵循的一种数据组装格式，只有服务端和客户端使用相同的协议，两者才能进行正常的数据交互，我们每天在网络中接触到的协议有HTTP，HTTPS等

### Loon支持以下协议
- ShadowSocks
- ShadowSocksR
- VMESS
    - VMESS + TLS
    - VMESS + WebSocket
    - VMESS + WebSocket + TLS
    - VMESS + HTTP
    - VMESS + HTTP + TLS
- VLESS
    - VLESS + WebSocket
    - VLESS + HTTP
- Trojan
    - Trojan + WebSocket
    - Trojan + HTTP
- HTTP
- HTTPS
- Wireguard
- Hysteria2
- Custom by JS

同时，Loon也支持使用JavaScript进行自定义代理协议，可参考[使用JS自定义HTTP代理](https://github.com/Loon0x00/LoonExampleConfig/blob/master/Script/http.js)

## 节点格式
如果要在配置文件中手动添加、修改单个节点，请遵从下面的格式
```
#ss
# 节点名称 = 协议，服务器地址，端口，加密方式，密码，fast-open=是否开启fast open（需要节点支持），udp=是否在UDP中使用（需要节点支持）
ss1 = Shadowsocks,example.com,443,aes-128-gcm,"password",fast-open=false,udp=true
ss2 = Shadowsocks,example2.com,443,chacha20,"password",fast-open=true,udp=true

#ss+simple obfs
# 节点名称 = 协议，服务器地址，端口，加密方式，密码，混淆方式=http|tls，obfs-host=混淆host，obfs-uri=混淆路径，fast-open=是否开启fast open（需要节点支持），udp=是否在UDP中使用（需要节点支持）
ssObfs1 = Shadowsocks,example.com,80,aes-128-gcm,"password",obfs-name=http,obfs-host=www.micsoft.com,obfs-uri=/,fast-open=true,udp=true
ssObfs2 = Shadowsocks,example.com,443,aes-128-gcm,"password",obfs-name=tls,obfs-host=www.micsoft.com,obfs-uri=/,fast-open=true,udp=true

#ssr
# 节点名称 = 协议，服务器地址，端口，加密方式，密码，protocol = 协议，protocol-param = 协议参数，obfs=混淆，obfs-param=混淆参数，fast-open=是否开启fast open（需要节点支持），udp=是否在UDP中使用（需要节点支持）
ssr1 = ShadowsocksR,example.com,443,aes-256-cfb,"password",protocol=origin,obfs=http_simple,obfs-param=download.windows.com,fast-open=false,udp=true
ssr2 = ShadowsocksR,example.com,10076,aes-128-cfb,"password",protocol=auth_chain_a,protocol-param=9555:loon,obfs=http_post,obfs-param=download.windows.com,fast-open=false,udp=true
ssr3 = ShadowsocksR,example.com,10076,chacha20,"password",protocol=auth_aes128_md5,protocol-param=9555:loon,obfs=tls1.2_ticket_auth,obfs-param=download.windows.com,fast-open=false,udp=true
ssr4 = ShadowsocksR,example.com,10076,chacha20-ietf,"password",protocol=auth_aes128_sha1,protocol-param=9555:loon,obfs=plain,fast-open=false,udp=true

#http
# 节点名称 = 协议，服务器地址，端口，加密方式，密码
http1 = http,example.com,80
http2 = http,example.com,80,username,"password"

#https
# 节点名称 = 协议，服务器地址，端口，加密方式，密码，skip-cert-verify=是否跳过证书校验（默认否），tls-name=SNI
https1 = https,example.com,443
https2 = https,example.com,443,username,"password"
https3 = https,example.com,443,username,"password",skip-cert-verify=true,tls-name=example.com

#vmess+tcp
# 节点名称 = 协议，服务器地址，端口，加密方式，UUID，transport(传输方式)=tcp，alterId=alterId（默认0，表示开启aead）
vmess1 = vmess,example.com,10086,aes-128-gcm,"52396e06-041a-4cc2-be5c-8525eb457809",transport=tcp,alterId=0,over-tls=false

#vmess+tcp+tls
# 节点名称 = 协议，服务器地址，端口，加密方式，UUID，transport(传输方式)=tcp，alterId=alterId（默认0，表示开启aead），over-tls=是否启用TLS，tls-name=SNI，skip-cert-verify=是否跳过证书校验（默认否）
vmess2 = vmess,example.com,10086,aes-128-gcm,"52396e06-041a-4cc2-be5c-8525eb457809",transport=tcp,alterId=0,path=/,host=v3-dy-y.ixigua.com,over-tls=true,tls-name=example.com,skip-cert-verify=true

#vmess+ws
# 节点名称 = 协议，服务器地址，端口，加密方式，UUID，transport(传输方式)=ws，alterId=alterId（默认0，表示开启aead），path=websocket握手header中的path，host=websocket握手header中的host
vmess3 = vmess,example.com,10086,aes-128-gcm,"52396e06-041a-4cc2-be5c-8525eb457809",transport=ws,alterId=0,path=/,host=v3-dy-y.ixigua.com,over-tls=false

#vmess+wss
# 节点名称 = 协议，服务器地址，端口，加密方式，UUID，transport(传输方式)=ws，alterId=alterId（默认0，表示开启aead），path=websocket握手header中的path，host=websocket握手header中的host，over-tls=是否启用TLS，tls-name=SNI，skip-cert-verify=是否跳过证书校验（默认否）
vmess4 = vmess,example.com,10086,aes-128-gcm,"52396e06-041a-4cc2-be5c-8525eb457809",transport=ws,alterId=0,path=/,host=v3-dy-y.ixigua.com,over-tls=true,tls-name=example.com,skip-cert-verify=true

#vmess+http
# 节点名称 = 协议，服务器地址，端口，加密方式，UUID，transport(传输方式)=http，alterId=alterId（默认0，表示开启aead），path=httpheader中的path，host=httpheader的host
vmess5 = vmess,example.com,10086,aes-128-gcm,"52396e06-041a-4cc2-be5c-8525eb457809",transport=http,alterId=0,path=/,host=v3-dy-y.ixigua.com,over-tls=false

#vmess+http+tls
# 节点名称 = 协议，服务器地址，端口，加密方式，UUID，transport(传输方式)=http，alterId=alterId（默认0，表示开启aead），path=httpheader中的path，host=httpheader的host，over-tls=是否启用TLS，tls-name=SNI，skip-cert-verify=是否跳过证书校验（默认否）
vmess6 = vmess,example.com,10086,aes-128-gcm,"52396e06-041a-4cc2-be5c-8525eb457809",transport=http,alterId=0,path=/,host=v3-dy-y.ixigua.com,over-tls=true,tls-name=example.com,skip-cert-verify=true

#VLESS+tcp
# 节点名称 = 协议，服务器地址，端口，UUID，transport(传输方式)=tcp
VLESS1 = VLESS,example.com,10086,"52396e06-041a-4cc2-be5c-8525eb457809",transport=tcp,over-tls=false

#VLESS+tcp+tls
# 节点名称 = 协议，服务器地址，端口，UUID，transport(传输方式)=tcp，over-tls=是否启用TLS，tls-name=SNI，skip-cert-verify=是否跳过证书校验（默认否）
VLESS2 = VLESS,example.com,10086,"52396e06-041a-4cc2-be5c-8525eb457809",transport=tcp,path=/,host=v3-dy-y.ixigua.com,over-tls=true,tls-name=example.com,skip-cert-verify=true

#VLESS+ws
# 节点名称 = 协议，服务器地址，端口，UUID，transport(传输方式)=ws，path=websocket握手header中的path，host=websocket握手header中的host
VLESS3 = VLESS,example.com,10086,"52396e06-041a-4cc2-be5c-8525eb457809",transport=ws,path=/,host=v3-dy-y.ixigua.com,over-tls=false

#VLESS+wss
# 节点名称 = 协议，服务器地址，端口，UUID，transport(传输方式)=ws，path=websocket握手header中的path，host=websocket握手header中的host，over-tls=是否启用TLS，tls-name=SNI，skip-cert-verify=是否跳过证书校验（默认否）
VLESS4 = VLESS,example.com,10086,"52396e06-041a-4cc2-be5c-8525eb457809",transport=ws,path=/,host=v3-dy-y.ixigua.com,over-tls=true,tls-name=example.com,skip-cert-verify=true

#VLESS+http
# 节点名称 = 协议，服务器地址，端口，UUID，transport(传输方式)=http，path=httpheader中的path，host=httpheader的host
VLESS5 = VLESS,example.com,10086,"52396e06-041a-4cc2-be5c-8525eb457809",transport=http,path=/,host=v3-dy-y.ixigua.com,over-tls=false

#VLESS+http+tls
# 节点名称 = 协议，服务器地址，端口，UUID，transport(传输方式)=http，path=httpheader中的path，host=httpheader的host，over-tls=是否启用TLS，tls-name=SNI，skip-cert-verify=是否跳过证书校验（默认否）
VLESS6 = VLESS,example.com,10086,"52396e06-041a-4cc2-be5c-8525eb457809",transport=http,path=/,host=v3-dy-y.ixigua.com,over-tls=true,tls-name=example.com,skip-cert-verify=true

#trojan
# 节点名称 = 协议，服务器地址，端口，alpn=tls扩展，skip-cert-verify=是否跳过证书校验（默认否），tls-name=SNI，udp=是否在UDP中使用（需要节点支持）
trojan1 = trojan,example.com,443,"password",alpn=http1.1,skip-cert-verify=false,tls-name=example.com,udp=true

#trojan+ws
# 节点名称 = 协议，服务器地址，端口，alpn=tls扩展，transport(传输方式)=ws，path=websocket握手header中的path，host=websocket握手header中的host，skip-cert-verify=是否跳过证书校验（默认否），tls-name=SNI，udp=是否在UDP中使用（需要节点支持）
trojan2 = trojan,example.com,443,"password",transport=ws,path=/,host=micsoft.com,alpn=http1.1,skip-cert-verify=true,tls-name=example.com,udp=true

#trojan+http
# 节点名称 = 协议，服务器地址，端口，alpn=tls扩展，transport(传输方式)=http，path=httpheader中的path，host=httpheader的host，skip-cert-verify=是否跳过证书校验（默认否），tls-name=SNI，udp=是否在UDP中使用（需要节点支持）
trojan2 = trojan,example.com,443,"password",transport=ws,path=/,host=micsoft.com,alpn=http1.1,skip-cert-verify=true,tls-name=example.com,udp=true

#Wireguard
wireguardNode = wireguard,interface-ip=192.168.2.2,interface-ipV6=2402:4e00:1200:ed00:0:9089:6dac:96b6,private-key="qF22B3ezOhWGJA4SHwQSsgMa9d6mPGHyFdZMaDTae2E=",mtu=1280,dns=192.168.2.1,dnsV6=2402:4e00:1200:ed00:0:9089:6dac:96b6,keeyalive=45,peers=[{public-key="JFuTIJEcFnt8R04UnAE5o2WfIPJUsumSxsD2ayXzoWY=",preshared-key="yVNv5K05AwVnWaR4OB8BlMX3jJlkS74aKlYC3PD95IE=",reserved=[1,2,3],allowed-ips="0.0.0.0/0",endpoint=192.168.3.17:51820}]

#Hysteria2
# 节点名称 = 协议，服务器地址，端口，密码，skip-cert-verify=是否跳过证书校验（默认否），tls-name=SNI，udp=是否在UDP中使用（需要节点支持），fast-open=是否开启fast open
hysteria2Node = Hysteria2,example.com,9898,"password",skip-cert-verify=true,tls-name=example.com,udp=true,fast-open=true

#js custom
# 节点名称 = 协议，服务器地址，端口，script-path=脚本路径（本地脚本直接为文件名，远端脚本为url）
jsHTTP = custom,192.168.1.139,6152,script-path=http.js
```

除了可以解析官方定义的节点格式，Loon也可以解析大部分服务提供商所提供的订阅节点，如遇到不支持的情况可以使用节点订阅解析脚本进行解析，目前常用的解析脚本由[SubStore](https://github.com/sub-store-org)提供，可在配置文件的general模块下进行如下配置，在之后的添加订阅节点页面开启解析器即可。
```
resource-parser = https://raw.githubusercontent.com/Peng-YM/Sub-Store/master/backend/dist/sub-store-parser.loon.min.js
```