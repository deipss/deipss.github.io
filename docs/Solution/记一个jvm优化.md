---
layout: default
title: 记一次jvm优化
parent: Solution
nav_order: 6
---

# 1. 背景
各个业务主机通过kafka将数据上报，有一个进行数据同步的机器，将kafka上的数据同步到消费，简单加工后，存放在ES。 
问题：4台2C16G的机器内存使用占比在85%以上。

# 2. 分析过程
- jps

选出进程

- jinfo

查看进程的信息
发现14G的jvm内存，新生代只占了140MB，许多数据全在老年代，且不回收。
后面发现是因为公司默认使用ParNew对年轻代进行回收，CMS进行老年代回收。140M是
公司默认的。

- jstat gcutil 5000 20
查看到有YGC频繁，每次30ms、FGC2小时一次

- jstat gcnew 5000 20
- jstat gcnewcapbility 5000 20

# 3. 解决办法
- 可以指定新生代最大的内存
- 使用G1作为垃圾回收

# 4. 参考资料
- https://zhuanlan.zhihu.com/p/83804324 JVM之G1回收器和常见参数配置
- https://zhuanlan.zhihu.com/p/626362331 JVM性能调优常用命令Jstat
- https://www.elastic.co/guide/en/logstash/current/tuning-logstash.html#profiling-the-heap
- https://www.elastic.co/guide/en/logstash/current/jvm-settings.html