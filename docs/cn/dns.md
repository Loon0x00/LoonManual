# DNS

Loon（截止Build 427）支持四种DNS查询，
- 标准UDP查询
- DNS-over-HTTPS
- DNS-over-QUIC
- DNS-over—HTTP3

## 相关配置

```
[General]
# dns服务，system表示系统自带dns服务器
dns-server = system,119.29.29.29,223.5.5.5
# DoH server，标准的url格式，以,分割多个地址
doh-server = https://example.com/dns-query
# DoQ server，以quic://开头，以,分割多个地址，默认端口784
doq-server = quic://example.com:784
# DoH3 server，标准的url格式，以,分割多个地址
doh3-server = h3://example.com/dns-query
```

## DNS查询逻辑
Loon将所有DNS查询分为两类，第一类是常规DNS查询，第二类是加密DNS查询（DoH/DoQ/DoH3），在同时配置了加密DNS和常规DNS服务器时，只会进行加密DNS查询，会并发向所有有效的DNS服务器发起查询，使用响应最快的查询结果。

## DNS缓存
Loon设计了两种DNS缓存，第一种是内存缓存，采用LRU算法缓存100条数据（iOS15后增加到200），在Loon启动过程中有效，关闭后缓存全部清除；第二种是磁盘缓存，根据TTL是否过期进行自动删除。同时允许关闭磁盘缓存逻辑。

## DNS查询、缓存命中逻辑
在对一个域名进行查询前，会在内存缓存中查询，如果命中缓存，会直接使用缓存结果，如果缓存IP的TTL过期，会进行一次查询，并使用查询结果更新缓存。如果没有命中内存缓存，会进一步查询磁盘缓存，磁盘缓存命中逻辑和内存缓存命中逻辑一致。如果没有缓存，会根据配置的DNS服务器并发查询，使用最先响应的结果，并更新缓存。

## 查询回落
在使用加密的DNS查询IP时，如果查询失败将会使用常规的DNS进行查询，可以在App的DNS服务器页面进行关闭