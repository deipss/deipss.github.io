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


# 3. explain

`EXPLAIN` 是 MySQL 中用于分析查询执行计划的命令。当你对一个 SQL 查询使用 `EXPLAIN` 时，MySQL 会返回查询的执行计划，而不是执行查询本身。这可以帮助你了解查询是如何被 MySQL 优化的，以及查询的性能瓶颈在哪里。

使用 `EXPLAIN` 可以帮助你：

1. **确定查询是否使用了索引**：如果查询没有使用索引，那么它可能会非常慢，尤其是当数据量很大时。
2. **查看查询的类型**：例如，是 `ALL`（全表扫描）、`index`（全索引扫描）还是 `range`（范围扫描）等。
3. **查看哪些列被使用**：这可以帮助你确定是否所有的列都被正确地使用了。
4. **确定哪些索引被使用**：这可以帮助你了解是否应该为特定的列添加索引。
5. **检查查询的行数**：`EXPLAIN` 会估计为了执行查询需要扫描的行数，这可以帮助你预测查询的性能。
6. **检查可能的额外开销**：例如，`Using filesort` 或 `Using temporary` 表明查询可能需要额外的排序或临时表，这可能会降低性能。

**如何使用 `EXPLAIN`**:

假设你有一个名为 `users` 的表，并且你想查询年龄大于 25 的所有用户：


```sql
EXPLAIN SELECT * FROM users WHERE age > 25;
```
执行上述查询后，你会得到一个结果集，其中包含了关于查询执行计划的详细信息。

**常见的 `EXPLAIN` 输出列**：

* **id**: 查询的标识符。
* **select_type**: 查询的类型（例如 SIMPLE, SUBQUERY, DERIVED 等）。
* **table**: 输出的表名。
* **type**: 访问类型（例如 ALL, index, range, ref, eq_ref, const, system, NULL）。
* **possible_keys**: 可能使用的索引。
* **key**: 实际使用的索引。
* **key_len**: 使用的索引的长度。
* **ref**: 哪些列或常量被用作索引查找的参考。
* **rows**: 估计需要检查的行数。
* **Extra**: 额外的信息，如 "Using where", "Using index", "Using temporary", "Using filesort" 等。

为了获得最佳的性能，你应该经常使用 `EXPLAIN` 来检查你的查询，并根据其建议进行优化。