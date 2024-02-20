---
layout: default
title: mysql锁与事务
parent: Database
nav_order: 1
---

# 锁

## 锁的分类

行锁
表锁(InnoDB中，意向锁即表级别的锁)

![img.png](img/innoDB_lock.png)

## 锁的算法

- 行锁：单行记录上锁
- 间隙锁：锁定一个范围，但不包括本身
- Next-Key锁=行锁+间隙锁（为了应对幻读的问题）