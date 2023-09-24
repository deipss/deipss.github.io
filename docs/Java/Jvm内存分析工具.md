---
layout: default
title: Jvm内存分析
parent: Java
---


使用以下命令进行dump java文件
```shell
jmap -dump:[live],format=b,file=<file-path> <pid>
jmap -dump,format=b,file=<file-path> <pid>
```

# eclipse mat

常见的几种报告


> 浅堆（Shallow Heap）和深堆（Retained Heap）是两个非常重要的概念，它们分别表示一个对象结构所占用的内存大小和一个对象被GC回收后，可以真实释放的内存大小。


# jvisualvm
