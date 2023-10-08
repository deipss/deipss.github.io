---
layout: default
title: Redis网络模型
parent: Database
---

图片来源：https://www.bilibili.com/video/BV1cr4y1671t?p=160&vd_source=f52d9488d7d3c21ed33580e4dce1a022

# 用户空间与内核空间

![img.png](img/user-system-space.png)

![img.png](img/user-system-space3.png)

# 阻塞IO

![block-io.png](img%2Fblock-io.png)

# 非阻塞IO

![non-block-io.png](img%2Fnon-block-io.png)

# IO多路复用

![img.png](img/multiple-io.png)

![img.png](img/multiple-io-2.png)

## select

![img.png](img/io-select.png)

## poll

![img.png](img/io-poll.png)

## epoll

![img.png](img/io-epoll.png)

解决的问题：

- 使用红黑树保存要监听的FD
- 只拷贝一次FD到内核空间
- 内核将就绪的FD直接拷贝到用户空间，不再用户空间自己去遍历出哪个FD是就绪的

![io-epoll-LT.png](img%2Fio-epoll-LT.png)
