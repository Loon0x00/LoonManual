# 复写
复写是专门用来处理HTTP/HTTPS类型的请求，在请求未发出前，根据所设定的复写类型来修改请求数据，目前可修改URL和Header

**复写的处理会在规则匹配之前**

## URL 类型复写
此类复写会修改请求的URL
```
^http://www\.google\.cn header http://www.google.com
```
## 直接响应类复写
此类复写直接返回一个假的响应
### 302
```
^http://example.com 302 https://example.com
```

### 307
```
^http://example.com 307 https://example.com
```

### reject
```
 ^http://example.com reject
 ^http://example.com reject-200
 ^http://example.com reject-img
 ^http://example.com reject-dict
 ^http://example.com reject-array
```

## Header 类型复写
此类复写会修改请求的Header
```
 ^http://example.com header-add Connection keep-alive
 ^http://example.com header-del Cookie
 ^http://example.com header-replace User-Agent Unknown
```