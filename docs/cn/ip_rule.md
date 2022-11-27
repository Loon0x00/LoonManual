# IP类规则

## IPV4
```
IP-CIDR,118.89.204.198/32,no-resolve
```

## IPV6
```
IP-CIDR6,2402:4e00:1200:ed00:0:9089:6dac:96b6/128
```

## GEOIP
根据mmdb查询的IP国家地区进行匹配
```
geoip,cn,DIRECT
```

## IP-ASN
根据IP服务商进行匹配
```
IP-ASN,4134,DIRECT,no-resolve
```

**no-resolve 可选**: 当设置no-resolve后表示该规则只会对目标地址类型是IP类型的生效，域名类型的目标地址进行dns解析后再去匹配这个规则