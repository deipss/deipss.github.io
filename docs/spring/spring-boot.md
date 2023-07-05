---
layout: default
title: spring-boot
parent: Spring
nav_order: 3
---

# 1. Auto Configuration

```text
找到 spring-boot-autoconfigure-2.3.4.RELEASE.jar 这个jar包，下面的META-INFO文件夹
找到spring.factories文件
在这个文件中搜索相关的关键字，如kafka，可以看kafka自动配置的入口类
```
自定义的 @Configuration如下

![auto-config-metainfo.png](..%2F..%2Fassets%2Fimg%2Fspring%2Fauto-config-metainfo.png)

## 1.1. Kafka

- CommonClientConfigs org.apache.kafka.clients.CommonClientConfigs

## 1.2. Log4j

```text
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/C:/Users/deips/.m2/repository/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/C:/Users/deips/.m2/repository/org/slf4j/slf4j-log4j12/1.7.30/slf4j-log4j12-1.7.30.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/C:/Users/deips/.m2/repository/org/apache/logging/log4j/log4j-slf4j-impl/2.13.3/log4j-slf4j-impl-2.13.3.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [ch.qos.logback.classic.util.ContextSelectorStaticBinder]
找到了多个binding类，将不需要去除
```

## 1.3. Dubbo

### 1.3.1. Dubbo 3.0

dubbo>curator>zookeeper
这样的一种调用关系


## 1.4. Redis

从spring-boot-autoconfigure-2.3.4.RELEASE.jar中找到了redis相关的配置类
```shell
org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisRepositoriesAutoConfiguration,\
```
其中RedisAutoConfiguration中，用到了几个关键类  **RedisProperties** **JedisConnectionConfiguration**
```shell
@Configuration(
    proxyBeanMethods = false
)
@ConditionalOnClass({RedisOperations.class})
@EnableConfigurationProperties({RedisProperties.class})
@Import({LettuceConnectionConfiguration.class, JedisConnectionConfiguration.class})
public class RedisAutoConfiguration {
```

而**JedisConnectionConfiguration**类中又使用到了如下类
```shell
@Configuration(
    proxyBeanMethods = false
)
@ConditionalOnClass({GenericObjectPool.class, JedisConnection.class, Jedis.class})
class JedisConnectionConfiguration extends RedisConnectionConfiguration {
    JedisConnectionConfiguration(RedisProperties properties, ObjectProvider<RedisSentinelConfiguration> sentinelConfiguration, ObjectProvider<RedisClusterConfiguration> clusterConfiguration) {
        super(properties, sentinelConfiguration, clusterConfiguration);
    }

    @Bean
    @ConditionalOnMissingBean({RedisConnectionFactory.class})
    JedisConnectionFactory redisConnectionFactory(ObjectProvider<JedisClientConfigurationBuilderCustomizer> builderCustomizers) throws UnknownHostException {
        return this.createJedisConnectionFactory(builderCustomizers);
    }
```