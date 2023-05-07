---
layout: default
title: sql
nav_order: 4
---

# mysql进程排查

```roomsql
show open tables where in_use > 0;
select * from information_schema.innodb_trx;
show processlist 
```
