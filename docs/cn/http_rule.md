# HTTP类规则
HTTP类型的规则只会对HTTP、HTTPS类型的请求进行匹配

## URL-REGEX
根据提供的正则表达式对请求的url进行匹配
```
URL-REGEX,^http://google\.com,PROXY
```
## USER-AGENT
根据请求header中的user-agent进行匹配，支持通配符
```
USER-AGENT,Apple*,DIRECT
```