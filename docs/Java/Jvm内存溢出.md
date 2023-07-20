---
layout: default
title: Jvm内存溢出
parent: Java
nav_order: 9
---

# 1 虚拟机栈与本地方法栈溢出

## 1-1 StackOverflowError

方法一 使用Xss 参数减少栈内存容量

方法二 在一个方法中，定义大量的本地变量，使用方法栈帧容量增大

## 1-2 OutOfMemoryError

循环创建线程，会出现提示

```shell
unable to create native thread
```


 