---
title: 使用 OC Runtime 实现动态方法调用
date: 2018-06-25 11:02:00
tags: [iOS, Runtime]
---

## 背景
实际项目开发时，或多或少都会需要用到动态方法调用。我在项目开发中有两个场景使用了动态方法调用：
- 想使用某个基础库的功能，但是改库并没有暴露头文件
- 使用某个基础库，但是不希望在编译期间引入对这个库的依赖

动态方法调用的适用场景还有更多，比如我们想要使用系统的私有方法，也可以通过动态方法调用的机制。不过这样有审核风险，所以一般不推荐使用。

Runtime是 OC 特有的机制，实际上也有很多开源库中，大量地使用 Runtime 来实现各种需求，比如大名鼎鼎的JSPatch，YYModel 等。本文我主要总结一下怎么使用 Rumtime 实现动态方法调用。

## 动态方法调用

### 基础知识
什么是动态方法调用？我们不妨回忆一下，通常情况下我们是怎么调用OC类的实例方法。看上去比较简单：初始化了一个Person实例，然后直接调用Person类的eat方法。

```objective-c
Person *p = [[Person alloc] init];
[p eat];
```
再看一下对应的底层实现：

```objective-c
[p performSelector:@selector(eat)];
objc_msgSend(p, @selector(eat));
```
主要流程：
- 查找selector
- 消息发送 objc_msgSend

### 实例方法调用流程
代码片段:
```objective-c
Class Person =NSClassFromString(@"Person");
if (Person) {    
    id person = [[Person alloc] init];
    if (person == nil) {
        return;
    }    
    NSMethodSignature *eat = [person methodSignatureForSelector:NSSelectorFromString(@"eat:")];
    NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:eat];
    int cnt = 10;
    [invocation setArgument:&(cnt) atIndex:2];
    invocation.selector = NSSelectorFromString(@"eat:")
    invocation.target = person;
    [invocation invoke];
}
```
流程总结:
- 获取 Class 定义 (NSClassFromString)
- 实例化 Class
- 定义实例方法签名 (methodSignatureForSelector)
- 创建 NSInvocation (invocationWithMethodSignature)
- 设置 invocation 参数 (setArgument，index从2开始)
- 获取待调用方法 selector (NSSelectorFromString)
- 设置 invocation 的 selector
- 设置 invocation 的 target (Class 实例)