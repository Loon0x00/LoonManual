# 订阅规则
订阅规则是一系列规则的集合，只要是满足Loon类型的规则都可以放入规则集中。
```
https://raw.githubusercontent.com/Loon0x00/LoonExampleConfig/master/Rule/ExampleRule.list, PROXY
```

## 查询性能
Loon目前可以承载百万级别数量的规则，无须担心性能和耗时问题。

目前测试情况如下：（3.1.7 build 669 iPhone15 Pro）
- 10万+ DOMAIN类型的规则查询时间：5ms内
- 10万+ IPCIDR规则查询时间：5ms内
- 10万+ IPCIDR6规则查询时间：20ms内

