---
title: YYKit源码学习-YYModel
date: 2018-06-08 11:03:25
tags: iOS
---

YYModel是一个高性能的 iOS JSON 模型框架，如果让我设计类似的框架，我能考虑到的几个关键点或者需要解决的问题有这几个：
- 怎么实现JSON/Dictionary与Model的互相转换
- 怎么保证类型安全
- 如何做到对Model代码无侵入
- Model嵌套如何支持
- 如何设计性能测试benchmark
- 性能方面的坑点

<!-- more -->

目前我能想到的问题只有这几个，当然实际编码时一定有更多细节。带着这些问题，可以开始看一发源码了。

## 代码结构
首先看一下代码结构，只有4个文件，代码量相对比较小。简单看一下源码，接口应该是在NSObject+YYModel里面定义和实现的，头文件中有大量的注释，包括接口的使用姿势、每个方法的详细解释，这点非常值得学习，YYClassInfo里面大量使用oc的runtime，获取class的method，SEL和IMP

![](./YYKit源码学习-YYModel/yymodel.png)

因为iOS基础比较薄弱，我先把源码中使用到的语言特性和知识点粗略地列出来，以备查阅：
- KVC
- Coding/Copying/hash/equal
- Category
- oc runtime

## 怎么实现JSON/Dictionary与Model的互相转换
### JSON/Dictionary转成Model

先看一下JSON/Dictionary怎么转成Model的，大概的流程如下图，橙色方框是对外部的接口。从流程图中可以看出来JSON是先转成Dictionary，然后复用modelWithDictionary的逻辑。

![](./YYKit源码学习-YYModel/modelwith.png)

### Model转成JSON/Dictionary

## 怎么保证类型安全

## 如何做到对Model代码无侵入

## Model嵌套如何支持

## 如何设计性能测试benchmark

## 用到了哪些语言特性

## 性能相关的Tips