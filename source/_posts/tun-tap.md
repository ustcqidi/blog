---
title: TUN & TAP
date: 2021-05-24 23:04:02
tags: [网络]
---

最近在看 “Let's Code Your own TCP/IP Stack” 系列文章，有点类似 “自己动手写 TCP/IP” 协议栈之类的。乍一看貌似一个不可能完成的任务，但是作者通过 5 篇文章逐步讲解，我居然看懂了。

<!--more-->

这其中大多数都是介绍网络协议的实现细节，比如 ARP 的 Header 格式、解析，TCP 的三次握手细节，重传算法，拥塞控制机制的实现等。我个人比较感兴趣的是，应用层如何拿到原始的网络数据包？

作者开篇就介绍了，使用 TUN & TAP 即可。晚上查阅了一些资料，真是打开了一扇门的感觉。原来我们平时使用的 VPN, 虚拟化技术之类的都得依赖 TUN & TAP。

这篇文章零散记录了 TAP 和 TUN 的一些资料。

## 什么是 TUN/TAP

> TUN/TAP provides packet reception and transmission for user space programs.

TUN 和 TAP Linux 内核的虚拟网络设备 

- TUN 是 network tunnel driver 的缩写
- TAP 是 ***Test Anything Protocol*** 的缩写

TUN 和 TAP 在网络栈中的关系如图：

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/418ae0e8-cf80-4cbc-8c5f-df72fe6f2ef2/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210524%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210524T151315Z&X-Amz-Expires=86400&X-Amz-Signature=951d7993e5024d3c045da9b1c3b6ed6fa891cdcadd7acff1efbf76d7444c610c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

## TUN 和 TAP 的应用场景

- The first one is **VPN** software (such as OpenVPN). In this scenario, the kernel sends its network packets to the tun or tap devices. The VPN software will then encrypt and forward them to the other side of the VPN tunnel where they get decrypted and delivered to their destination.
- The second area in which tun and tap devices are popular are system virtualization/emulation packages.
- NAT

## 实验

- OpenVPN 创建 tun
- 代码读取 tun 数据

## 参考资料

- [tuntap-interface-tutorial](https://backreference.org/2010/03/26/tuntap-interface-tutorial/)
- 虚拟化技术：libvirt & QEMU & KVM