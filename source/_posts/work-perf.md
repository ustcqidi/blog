---
title: 用脚本把繁杂、重复的工作自动化
date: 2020-03-14 20:29:59
tags: [工作效率, Python, 自动化]
---

用脚本把所有繁杂、重复的工作自动化，是提升工作效率和身心愉悦感的很棒的方法。还能大幅度地降低人为操作的失误。

<!--more-->

## JNIGenerator

Chromium 项目中大量地用 Python 脚步实现了各种工作的自动化，比如自动生成 JNI 接口。人肉写 JNI 接口很容易出错，需要记大量的 JNI 数据类型，非常痛苦。我把 Chromium 中的 JNI 接口生成以及相关基础 JNI 封装抽出来了。参考 [JNIGenerator](https://github.com/ustcqidi/JNIGenerator)

## ZMBuildDownloader

平时工作中每次更新项目代码，都要从4个仓库拉一遍，还得从我们的 Build FTP 上，取最新的 Binary 替换本地的。这个工作用脚本实现自动化最合适不过了。 除了更新代码，我还经常需要从 FTP 上取最新的 build 测试，或者取某个 build 的符号文件来分析一些 Crash 问题。

大体分析了一下需求以后，我写了一个简单的脚本，可以从 FTP 上获取最新或者指定版本的 build/bianry/symbols 文件，顺便写了个自动更新代码的脚本，两个脚本一搭配效率迅速提升。再也不用蛋疼地打开 FileZilla 眯着眼找 build 了。

## 批量删除 git 仓库本地分支

### 删除所有包含_trunk的本地分支

git branch | grep "_trunk" | xargs git branch -D

### 删除所有不包含trunk的本地分支

git branch | grep -v "trunk" | xargs git branch -D

## 日志清理脚本

一键清理垃圾日志文件，释放磁盘空间