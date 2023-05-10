---
layout: default
title: spring-boot
parent: Spring
nav_order: 3
---

# 1. auto configuration

```text
找到 spring-boot-autoconfig这个jar包，下面的META-INF文件夹
找到spring.factories文件
在这个文件中搜索相关的关键字，如kafka，可以看kafka自动配置的入口类
```
自定义的@Configuration如下

![img.png](..%2Fassets%2Fimg%2Fspring%2Fimg.png)
## 1.1. kafka

- CommonClientConfigs org.apache.kafka.clients.CommonClientConfigs

## 1.2. log4j

```text
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/C:/Users/deips/.m2/repository/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/C:/Users/deips/.m2/repository/org/slf4j/slf4j-log4j12/1.7.30/slf4j-log4j12-1.7.30.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/C:/Users/deips/.m2/repository/org/apache/logging/log4j/log4j-slf4j-impl/2.13.3/log4j-slf4j-impl-2.13.3.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [ch.qos.logback.classic.util.ContextSelectorStaticBinder]
找到了多个binding类，将不需要去除
```

## 1.3. dubbo

### 1.3.1. dubbo 3.0

dubbo>curator>zookeeper
这样的一种调用关系