---
layout: default
title: java
parent: Command
nav_order: 3
---

# 1. 高CPU使用率分析

## 1.1. alibaba arthas

```
1、top 命令分析，是否高CPU使用率、负载率，但是CPU空闲时间长
2、安装arthas
thread 查询有多少线程，及其CPU使用率
thread -n [tid] >> [filename] 将某个线程的执行方法栈及CPU状态字输出到某个文件
```

- 参考 https://www.jianshu.com/p/3ba1e933682b

## 1.2. Top命令结合 jstack

```
top 命令分析，是否高CPU使用率、负载率，但是CPU空闲时间长
top -Hp <pid> 查询pid中的线程
printf "%x\n" <tid>  把线程id转为16进制
jstack -l <pid> >> ./jstack_result.txt 将此时的jvm快照打印到指定txt文件
可以结合 | grep 来检索不同状态的线程
在txt文件中搜索16进制的线程id
```

- https://www.cnblogs.com/fengweiweicoder/p/10992043.html

# 2. 高内存分析

## 2.1. jmap 文件下载

```shell
打印出jvm进程堆使用情况
jmap -heap <pid>
下载快照到文件
jmap -dump:file=filename.dump <pid>
使用jhat命令
jhat -port 9998 filename.dump

在windows可以使用jvisualvm.exe命令，加载文件分析  
```

## 2.2. 内存对象统计排序

```
排序出目前容量最大的一些类，-k 2是根据第2列排序，就是数据最大的
jmap -histo <pid> | grep <class full path> | sort -n -k 3 | head 17
 num     #instances         #bytes  class name 
----------------------------------------------
   1:       4632416      392305928  [C
   2:       6509258      208296256  java.util.HashMap$Node
   3:       4615599      110774376  java.lang.String
   5:         16856       68812488  [B
   6:        278914       67329632  [Ljava.util.HashMap$Node;
   7:       1297968       62302464  

```

# gc日志

## gc 查看

```shell
# 3. 查看帮助
jstat -help 
# 4. 查看统计的可选项
jstat -option

# 5. 使用jstat命令查看gc情况 每5秒一次，统计20次，每5行显示一个表头
jstat -gcutil -h5 <pid> 5000 20

# 6. 使用jstat命令查看gc情况 每5秒一次，统计20次，每5行显示一个表头
jstat -gc -h5 <pid> 5000 20
 S0C: 当前幸存者区0的容量 (kB).
 S1C: 当前幸存者区1的容量(kB).
 S0U: 幸存者区0已用内存 (kB).
 S1U: 幸存者区1已用内存 (kB).
 EC: 伊甸园区容量 (kB).
 EU: 伊甸园区已用内存 (kB).
 OC: 当前老旧区容量 (kB).
 OU: 老旧区已用内存 (kB).
 MC: 元数据区容量 (kB).
 MU: 元数据区已用内存 (kB).
 CCSC: 类压缩区容量 (kB).
 CCSU: 类压缩区已用内存 (kB).
 YGC: 新生垃圾回收事件数量.
 YGCT: 新生垃圾回收时间.
 FGC: 垃圾回收事件总和.
 FGCT: 完整的一次垃圾回收时间.
 GCT: 所有的垃圾回收时间.

# 7. 查看到GC的内存占用情况
jstat -gccapbility
```
- 参考 https://zhuanlan.zhihu.com/p/481206194

## gc 日志打印
```shell
# 8. 必备
-XX:+PrintGCDetails 
-XX:+PrintGCDateStamps 
-XX:+PrintTenuringDistribution 
-XX:+PrintHeapAtGC 
-XX:+PrintReferenceGC 
-XX:+PrintGCApplicationStoppedTime

# 9. 可选
-XX:+PrintSafepointStatistics 
-XX:PrintSafepointStatisticsCount=1

# 10. GC日志输出的文件路径
-Xloggc:/path/to/gc-%t.log
# 11. 开启日志文件分割
-XX:+UseGCLogFileRotation 
# 12. 最多分割几个文件，超过之后从头文件开始写
-XX:NumberOfGCLogFiles=14
# 13. 每个文件上限大小，超过就触发分割
-XX:GCLogFileSize=100M
```

- https://segmentfault.com/a/1190000039806436

## javap 反编译代码
```shell
通过以下命令来反编译出一个Class文件字节码
javap -verbose -p Main.class
```

## G1配置
- https://zhuanlan.zhihu.com/p/83804324

# 使用Arthas

## 安装

```shell script
curl -O https://arthas.aliyun.com/arthas-boot.jar
java -jar arthas-boot.jar
```

## ognl

```bash
注意SpringExtensionFactory的版本，不同版本，类路径可能不一样
sc -d 'org.apache.dubbo.config.spring.extension.SpringExtensionFactory'
上面的命中得出classLoader的内存地址
ognl -c 2e1ef60 '#context=@org.apache.dubbo.config.spring.extension.SpringExtensionFactory@getContexts().iterator.next, 
context.getBean("umsTradeBillSplitJob").execute(null)' -x 3
可以使用new construct() 构造函数来声明一个变量 #a=new java.lang.Object(1)，注意使用要带上#号
```

## thread

```bash
# 14. 打印出阻塞的线程信息
thread -b
# 15. 统计最近 1000ms 内的线程 CPU 时间
thread -i 1000
# 16. 列出 1000ms 内最忙的 3 个线程栈
thread -n 3 -i 1000 

```

- 参考 https://cloud.tencent.com/developer/article/1846725

## monitor

```shell script
# 17. 每秒的请求数
monitor -c 1 <类全路径名> <方法名>
```

## trace

```shell script
# 18. 方法内部调用路径，并输出方法路径上的每个节点上耗时
# 19. 可以指定毫秒数
trace com.frxs.repeater.receiver.event.consumer.RecieveGeneralMsgConsumer onMessage  -n 5 --skipJDKMethod false '#cost > 3000'
```

## classloader

```shell
# 20. 按类加载实例查看统计信息
classloader -l
# 21. 查看 ClassLoader 的继承树
classloader -t
# 22. 查看 URLClassLoader 实际的 urls
classloader -c 3d4eac69
```

## profile

火焰图查看

```shell script
profiler start
profiler status
profiler stop --format html
```

# jvm启动获取参数

- 系统主机中的参数
- jvm 进程启动时参数
- main 函数中获取