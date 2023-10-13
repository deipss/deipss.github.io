---
layout: default
title: mysql
parent: Database
nav_order: 1
---

# 1. mysql efficient shell

## 1.1. show dead lock

```shell
show open tables where in_use > 0;
select * from information_schema.innodb_trx;
show processlist 
```

## 1.2. table structure copy

```shell
select  concat('drop  table if exsits frxs_fund_accountant_2012.', table_name ,';create  table  frxs_fund_accountant_2012.', table_name ,' like ','frxs_fund_accountant_2001.',table_name,';')  
from information_schema.tables WHERE table_schema='frxs_fund_accountant';
```

# 2. 索引

## 2.1. 回表

指通过索引查询命中主键ID，再返回到表中去读取ID对应的列数据（这些列数据不在索引中）

### 2.1.1. 聚集索引 （clustered index）

InnoDB聚集索引的叶子节点存储行记录，因此， InnoDB必须要有且只有一个聚集索引。

- 如果表定义了主键，则Primary Key 就是聚集索引；
- 如果表没有定义主键，则第一个非空唯一索引（Not NULL Unique）列是聚集索引；
- 否则，InnoDB会创建一个隐藏的row-id作为聚集索引；

### 2.1.2. 普通索引（secondary index）

普通索引也叫二级索引，除聚簇索引外的索引都是普通索引，即非聚簇索引。

- InnoDB的普通索引叶子节点存储的是主键（聚簇索引）的值，而MyISAM的普通索引存储的是记录指针。