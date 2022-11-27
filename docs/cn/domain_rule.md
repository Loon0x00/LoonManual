# 域名类规则

## DOMAIN
匹配整个域名
```
DOMAIN,google.com,proxy
```

## DOMAIN-SUFFIX
匹配域名后缀，例如`apple.com`可以匹配`icloud.apple.com`，`www.apple.com`，但是不能够匹配`app-apple.com`
```
DOMAIN-SUFFIX,apple.com,proxy
```

## DOMAIN-KEYWORD
域名关键词匹配
```
DOMAIN-KEYWORD,apple,proxy
```
