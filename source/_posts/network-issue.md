---
title: 发出一个网络请求有多难
date: 2020-10-18 20:51:04
tags: [网络]
---

我们已经习惯各种 App 带来的便利：聊天、订外卖、叫车、刷抖音。其实看是简单的操作比如打开一个网站，发送一个表情，都涉及大量网络请求。完成一次网络请求其实也挺难的，我相信所有用户量大的 App 一定都做了很多应用层面的网络优化。

<!--more-->

## 网络请求流程
先看一下一个网络请求需要经历哪些流程，现在绝大多数的网站 / App 都是 https, 所以这里只描述 https 请求的流程。

首先是域名解析，通过域名查询对应的 host IP 地址；然后就是 TCP 连接这个 IP 地址；最后是 TLS 握手建立安全的数据通道，而后就可以收发数据了。

看上去整个流通也挺简单的，但是这里面每一步都有可能出现问题。

### DNS 
- 运营商劫持
- 域名污染
- 本地配置 host
- DNS 服务配置有问题

### TCP 
- 防火墙限制

### TLS
- SSL Inspector
- 自签名证书
- 抓包工具
- 代理服务器

## Proxy

### 配置
- PAC

### 认证
- BASIC
- Digest
- NTLM
- Kerbose