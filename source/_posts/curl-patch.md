---
title: 给 curl 提了一个 patch
date: 2020-10-18 20:16:23
tags: [网络, cURL]
---

最近把 curl 升级到了 7.71.1 版本，然后 NTLM 认证又又又又出问题了。问题表现是开启了抓包工具后 NTLM 认证就一直失败，给官方报了一个 Issue，[NTLM authentication fails when using proxy without username and password](https://github.com/curl/curl/issues/5911)

然后尝试给官方提了一个 Pull Request，最终被合到 master 分支。[Pull Request](https://github.com/curl/curl/pull/5914)

这是我第一次给开源社区贡献代码，还是挺激动的。

有几点收获这里记录一下：
- 发现开源项目问题，主动分析原因，思考解决方法
- 如果解决了问题，思考一下能否回馈开源社区，尽量找到 Root Cause，不要用 Workaround
- 通常开源项目都有自己的代码风格，提交 Pull Request 时要遵守项目的代码风格、通过各种静态检查。