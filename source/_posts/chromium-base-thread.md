---
title: Chromium 跨平台基础库：多线程
date: 2018-12-14 22:29:02
tags: [Chromium, 多线程, 基础库]
---

多线程是基础库非常重要的一部分，每个平台都有各自的多线程API，实现一套高效、易用的多线程基础库也是挺有挑战的。Chromium base库中有完整的跨平台thread封装和实现，本文主要整理一下Chromium 线程相关实现细节。

<!--more-->

## 如何阅读 Chromium 源码
Chromium 是个庞大的开源项目，完整地下载一份代码需要不少时间。就算代码下载到本地了，在配置一般的机器使用IDE打开的话，会卡到你怀疑人生。根本没法跟踪代码，比如想看一下某个函数的定义、实现或者引用等。推荐在 https://cs.chromium.org/ 这个网站(需要科学上网)上看代码，它支持模糊搜索, 类型的 declaration, definition, call hierarchy 和 history 等；只要你机器配置和网络还凑合，使用体验还是不错的。

## 代码结构
线程相关的代码在 base/threading 目录下，先大体看一下有哪些文件：

![](./chromium-base-thrad/code_overview.png)

看上去代码量较大，先大体看一下每个文件大概是干啥的：
- platform_thread 是对各个平台线程接口的抽象封装，platform_thread_$os 是针对平台的具体实现，比如 platform_thread_android。值得注意的是，不能直接使用 platform_thread
- thread.h/cc 是核心实现，抽象了线程的 MessageLoop 概念，thread 启动时会同时启动一个 Runloop，MessageLoop 实现 Runloop 的 Delegate 方法；具体的执行任务封装成 Task 放入 MessageLoop 中执行
- MessageLoop, Task 等代码分别在 base/message_loop 和 base/task 下面
- 线程同步/锁 相关代码在 base/synchronized 下面
- simple_thread.h/cc 是对各原生平台线程接口的简单封装，没有引入 MessageLoop 可以直接使用，伪代码如下：
```c++
// Example:
class MyThreadRunner : public DelegateSimpleThread::Delegate { ... };
MyThreadRunner runner;
DelegateSimpleThread thread(&runner, "good_name_here");

// Start will return after the Thread has been successfully started and
// initialized.  The newly created thread will invoke runner->Run(), and
// run until it returns.
thread.Start();

// Wait until the thread has exited.  You *MUST* Join!
thread.Join();
```
Chromium thread 的主要类以及他们之间的关系如下图：

![](./chromium-base-thrad/thread_class.png)

剩下的都时一些辅助的工具类和类型定义：
- thread_checker 用来检查是否在同一个线程调用
- scoped_blocking_call 描述任何阻塞线程的调用，比如同步文件I/O等
- sequence_bound 
- thread_id_name_manager 线程 id 和 名字 的映射关系
- thread_collision_warner 定了了一些用于线程安全检测的宏
- watch_dog 创建了一个辅助线程，检测一段时间内某个行为是否没有响应，比如可以检测线程是不是挂起了
- thread_restrictions 禁止或允许某些耗时的操作
- thread_local / sequence_local_storage_slot 存取

## 一些启发
- 基础库需要充分的单元测试，一方面保证代码质量，另一方面单元测试是很好的 code example，方便别人了解如何使用基础库
- 诊断线程被挂起的方法


