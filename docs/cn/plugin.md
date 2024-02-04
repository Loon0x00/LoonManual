# 插件
插件是规则、复写、脚本的集合，相当于一个子配置，常常用来代表一个扩展功能。

## 插件可包含的配置模块
```
#!name= 插件名称
#!desc= 这是一个带有配置项的插件，input代表输入，select代表选择（select的第一项为名称，后面为可选值），用户所填或者选择的值都可以在脚本中用$persistentStore.read进行读取，如$persistentStore.read(appName)
#!author= 插件作者
#!homepage= 插件首页，可在插件页面进行跳转
#!icon= 插件的图标
#!author = Loon0x00
#!input = appName
#!input = author
#!select = appType,tool,social,health,sport
#!select = price,0.99,1.99,4.99


[General]
bypass-tun =
skip-proxy =
real-ip =
dns-server =

[rule]

[rewrite]

[host]

[script]

[mitm]

```

## 插件中规则的策略
插件内的规则指向的策略只能有如下三种，当规则不指定策略时，会默认使用DIRECT
1. DIRECT
2. REJECT类（REJECT，REJECT-IMG，REJECT-DICT，REJECT-ARRY，REJECT-DROP）
3. PROXY，代表用户在进行插件配置时手动选择的策略组，如果用户指定了PROXY，但插件却没有进行配置，那最终将按照无法找到策略组的逻辑进行处理（即使用App全局中第一个节点）

## 插件推荐
[Loon 插件仓库](https://github.com/Peng-YM/Loon-Gallery)(由热心网友整理提供)
