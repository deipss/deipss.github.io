---
layout: default
title: arthas
parent: Command
nav_order: 15
---

# 使用Arthas

## 安装

```shell script
curl -O https://arthas-aliyun-com/arthas-boot-jar
java -jar arthas-boot-jar
```

## ognl

```bash
# 注意SpringExtensionFactory的版本，不同版本，类路径可能不一样
sc -d 'org-apache-dubbo-config-spring-extension-SpringExtensionFactory'
# 上面的命中得出classLoader的内存地址
ognl -c 2e1ef60 '#context=@org-apache-dubbo-config-spring-extension-SpringExtensionFactory@getContexts()-iterator-next, 
context-getBean("umsTradeBillSplitJob")-execute(null)' -x 3
# 可以使用new construct() 构造函数来声明一个变量 #a=new java-lang-Object(1)，注意使用要带上#号
```

## thread

```bash
# 打印出阻塞的线程信息
thread -b
# 统计最近 1000ms 内的线程 CPU 时间
thread -i 1000
# 列出 1000ms 内最忙的 3 个线程栈
thread -n 3 -i 1000 

```

- 参考 https://cloud-tencent-com/developer/article/1846725

## monitor

```shell script
# 每秒的请求数
monitor -c 1 <类全路径名> <方法名>
```

## trace

```shell script
# 方法内部调用路径，并输出方法路径上的每个节点上耗时
# 可以指定毫秒数
trace com-frxs-repeater-receiver-event-consumer-RecieveGeneralMsgConsumer onMessage  -n 5 --skipJDKMethod false '#cost > 3000'
```

## classloader

```shell
# 按类加载实例查看统计信息
classloader -l
# 查看 ClassLoader 的继承树
classloader -t
# 查看 URLClassLoader 实际的 urls
classloader -c 3d4eac69
```

## profile

火焰图查看

```shell script
profiler start
profiler status
profiler stop --format html
```
