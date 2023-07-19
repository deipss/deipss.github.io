---
layout: default
title: mysql
parent: Database
nav_order: 1
---

# mysql进程排查

```shell
show open tables where in_use > 0;
select * from information_schema.innodb_trx;
show processlist 
```
