---
title: Objective-C Runtime 详解
---

## 简介

## 消息

## 动态方法解析
接口执行脚本，创建一个 ProgramExecutable 对象来表示要执行的脚本负责编译成 ByteCode，调用 Interpreter 执行这个 ByteCode。
### 动态方法解析
接口执行脚本，创建一个 ProgramExecutable 对象来表示要执行的脚本负责编译成 ByteCode，调用 Interpreter 执行这个 ByteCode。

Binding 是 WebCore 为 JavaScriptCore 提供的封装层。这一层的定义可以在这里找到：https://trac.webkit.org/wiki/WebKitIDL。WebKit 参照的是 W3C Web IDL https://www.w3.org/TR/WebIDL-1/。

使用 IDL 定义接口规范，WebKit 使用一组 perl 脚本来转换 IDL，初始脚本是 generate-binding.pl。生成接口与 DOM 组件关联是通过 JSNode 来的。执行脚本和 Frame 的 setDocument 会更新 document 对象到 JavaScriptCore，通过 JSDomWindowBase::updateDocument 更新到 JSC::Heap 里。
### 动态加载
接口执行脚本，创建一个 ProgramExecutable 对象来表示要执行的脚本负责编译成 ByteCode，调用 Interpreter 执行这个 ByteCode。

Binding 是 WebCore 为 JavaScriptCore 提供的封装层。这一层的定义可以在这里找到：https://trac.webkit.org/wiki/WebKitIDL。WebKit 参照的是 W3C Web IDL https://www.w3.org/TR/WebIDL-1/。

使用 IDL 定义接口规范，WebKit 使用一组 perl 脚本来转换 IDL，初始脚本是 generate-binding.pl。生成接口与 DOM 组件关联是通过 JSNode 来的。执行脚本和 Frame 的 setDocument 会更新 document 对象到 JavaScriptCore，通过 JSDomWindowBase::updateDocument 更新到 JSC::Heap 里。

## 消息转发

## 类型编码

## 属性