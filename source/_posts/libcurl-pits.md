---
title: libcurl 陷阱
date: 2020-03-27 23:09:58
tags: [网络, cURL]
---

记录项目使用 libcurl 时遇到的一些坑，持续更新！

<!--more-->

## 动态库卸载 Crash
libcurl 作为动态库一部分，在动态库卸载时如果有 pending 的 dns 解析请求可能会导致 Crash。
- [相关issue](https://github.com/curl/curl/issues/997)
- [官方文档说明](https://curl.haxx.se/libcurl/c/curl_global_cleanup.html)

>curl_global_cleanup does not block waiting for any libcurl-created threads to terminate (such as threads used for name resolving). If a module containing libcurl is dynamically unloaded while libcurl-created threads are still running then your program may crash or other corruption may occur. We recommend you do not run libcurl from any module that may be unloaded dynamically. This behavior may be addressed in the future.

## Connection Cache 复用问题
网络切换/中断、App前后台切换，可能会导致 Connect Cache 的 socket 连接实例失效，但是 libcurl 没有针对这种情况做 Connection Cache 的及时清理，到时连接复用时可能会出现连接失败。这个问题在 iOS, Mac 平台上很容易出现。
- [相关讨论](https://curl.haxx.se/mail/lib-2016-07/0032.html)

>I've got a situation where connection cache is kept through internet connection change (Wifi -> 3G for example). After network change cURL will try to reuse the connection from cache and will fail and open a new connection. The problem is that it takes ~20 seconds to understand that the connection was dead.