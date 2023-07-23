---
layout: default
title: Redis
parent: Database
nav_order: 3
---

# 主从复制

```shell


# 启动主从三个redis容器
docker run -it --name redis-6300 -d -p 6300:6379 redis redis-server
docker run -it --name redis-6380 -d -p 6380:6379 redis redis-server
docker run -it --name redis-6381 -d -p 6381:6379 redis redis-server

# 使用 docker inspect 查看主redis在docker中容器的内部IP
docker inspect redis-6300[容器名称或 id]

# 进入容器的内部，并配置从redis的主服务机ip和端口
docker exec -it redis-6300 bash
redis-cli
info replication

slaveof [172.17.0.6] 6300

# 配置哨兵
docker exec -it redis-6300 bash
cd /
apt-get update
apt-get install -y vim
vim sentinel.conf   -> sentinel monitor host6379 172.17.0.6 6379 1
redis-sentinel sentinel.conf
对三个redis容器都进行上述操作
```

# 持久化

## RDB

在的指定的时间间隔内，将内存中的数据以快照的方式存入磁盘，恢复时将快照文件直接读入到内存
redis会单独创建（fork）一个子进程来进行持久化，会将数据暂时写入到一个临时文件中，待持久化的过程全部结束，将这个临时文件替换为新的.rdb文件。整个过程中，主进程不进行任何IO操作，这就确保了Redis整体性能。
其配置在【SNAPSHOTTING】一栏中

## AOF

以日志的形式来记录每个写操作，将redis执行过的所有写指令保存，只是追加文件而不是改写文件，redis启动时会加载这个.aof文件重新构建数据。为避免.aof文件累加过大，redis会以重写的形式来更新.aof文件的版本，保持数据的
其配置中【APPEND ONLY MODE】一栏中

### fork

是将当前进程复制出一个一模一样的进程出来，包括当前进程的变量、程序计算器、上下文环境，这个全新的进程作为当前进程的子进程。

# 数据类型

## reids中的key

- keys * ：列出当前库中所有的key
- exists 为：判断key_name是滞存在
- expire key_name [s]：给key_name设置过期时间
- ttl key_name：查看还有多少秒过期 -1表示永不过期、-2表示已过期、0表示正在计时
- type key_name：查看key_name的类型

## String

- set/get/del/append/strlen
- incr/decr/incrby/decrby :后者是按步增长、减少
- getrange/setrange
- setex(set with expire)：
- setnx(set if not exists)
- mset/mget/msetnx
- getset

## List

## Set

- sadd/smembers/sismenber
- scard 获取集合里面的元素个数
- srem key value：删除集合中的元素
- srandmember key [int] 随机选取几个元素
- spop key ：随机出栈
- smove key1 key2：将key1的值赋予key2
- sdiff\sinter\sunion：差集、交集、并集

## Zset

- zadd/zrange
- zrangebyscore key [开始的分数] [结束的分数]
- zrem key 删除元素
- zcard/zcount key score区间/zrank key vlaues ：
- zrevrank key values ：逆序获得下标值
- zrevrange
- zrevrangebyscore key [开始的分数] [结束的分数]

## Hash

- hset/hget/hmset/hmget/hgetall/hdel
- hlen
- hexists key
- hkeys/hvals
- hincrby/hincbyfloat
- hsetnx

# 缓存问题

## 击穿

## 雪崩

## 踩踏

当多个线程试图并行访问缓存时，就会发生缓存踩踏。如果缓存的值不存在，那么线程将同时尝试从数据源获取数据。数据源通常是数据库，也可以是
Web 服务器、第三方 API 或任何其他可以返回数据的东西。
缓存踩踏之所以极具破坏性，一个主要原因是它会导致恶性的失败循环：

1. 大量的并发线程无法从缓存中获得数据，然后直接调用数据库。
2. 数据库由于巨大的 CPU 峰值发生崩溃，并导致超时错误。
3. 收到超时错误后，所有的线程都会发起重试，从而导致另一次踩踏。
4. 这个循环不断持续。
    - https://www.infoq.cn/article/Bb2YC0yHVSz4qVwdgZmO