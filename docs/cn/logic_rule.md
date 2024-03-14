# 逻辑规则
使用或、与、非逻辑将多个规则合并成一个规则（3.1.7+）

**如果逻辑规则里面有域名有IP，尽量把IP的子规则放在后面，防止不必要的DNS查询**

## AND
多个子规则同时满足时才会匹配

**AND,((子规则),(子规则)),PolicyName**
```
AND,((DOMAIN-SUFFIX,axample),(DEST-PORT,443),(GEOIP,CN)),DIRECT
```

## OR
子规则满足一个时匹配

**OR,((子规则),(子规则)),PolicyName**
```
OR,((DOMAIN-SUFFIX,axample),(DEST-PORT,443),(GEOIP,CN,no-resolve)),DIRECT
```

## NOT
子规则不满足时匹配，只有有一个子规则

**NOT,((子规则)),PolicyName**
```
NOT,((AND,((DOMAIN-SUFFIX,axample),(DEST-PORT,443),(GEOIP,CN)))),DIRECT
```