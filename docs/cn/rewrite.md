# 复写
复写是专门用来处理HTTP/HTTPS类型的请求，在请求未发出前，根据所设定的复写类型来修改请求数据，目前可修改URL和Header，**所有的复写仅针对http请求或者经过解密后的https请求有效**

**复写的处理会在规则匹配之前**

## URL 类型复写
此类复写会修改请求的URL
```
^http://www\.google\.cn header http://www.google.com
```
## 直接响应类复写
此类复写直接返回一个code位30x的重定向response
### 302
```
^http://example.com 302 https://example.com
```

### 307
```
^http://example.com 307 https://example.com
```

### reject
1. **reject**: 直接断开连接
2. **reject-200**: 返回一个code为200，body内容为空的response
3. **reject_img**: 返回一个code为200，body内容一像素图片的的response
4. **reject_dict**: 返回一个code为200，body内容为`"{}"`的空json对象字符串
5. **reject_array**: 返回一个code为200，body内容为`"[]"`的空json数组字符串
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