---
title: mysql-join
date: 2020-05-14 11:31:53
tags: mysql
---

## 连接简介

###### 准备两张表 `t1` `t2`

```sql
CREATE TABLE t1 (a1 int , b1 char(2));

CREATE TABLE t2 (a2 int , b2 char(2));
```

###### 插入演示数据

```sql
INSERT INTO t1 VALUES (1,'aa'),(2,'bb'),(3,'cc');
INSERT INTO t2 VALUES (11,'aa'),(22,'bb'),(23,'cc');
```

执行以上步骤我们已经创建表`t1` `t2`, 并且已经插入数据。

```sql

mysql> SELECT * FROM t1;
+----+----+
| a1 | b1 |
+----+----+
|  1 | aa |
|  2 | bb |
|  3 | cc |
+----+----+
3 rows in set (0.02 sec)

mysql> SELECT * FROM t2;
+----+----+
| a2 | b2 |
+----+----+
| 11 | aa |
| 22 | bb |
| 23 | cc |
+----+----+
3 rows in set (0.03 sec)

mysql> 

```

连结就是将每个表中的数据取出来一次组合匹配的结果返回给用户。

连结之后的结果就是：
```sql

mysql> SELECT * FROM t1, t2;
+----+----+----+----+
| a1 | b1 | a2 | b2 |
+----+----+----+----+
|  1 | aa | 11 | aa |
|  2 | bb | 11 | aa |
|  3 | cc | 11 | aa |
|  1 | aa | 22 | bb |
|  2 | bb | 22 | bb |
|  3 | cc | 22 | bb |
|  1 | aa | 23 | cc |
|  2 | bb | 23 | cc |
|  3 | cc | 23 | cc |
+----+----+----+----+
9 rows in set (0.04 sec)


```

> 笛卡尔积
> 如果我们连结多张表操作，没有限制条件的话，产生的笛卡尔积可能会非常巨大（t1_data_line * t2_data_line * ...）


###### 连接过程

1. 第一步：第一个需要查询的表称为`驱动表`，在 `SELECT * FROM t1, t2;` 中`t1` 是驱动表
2. 第二步：对于上一步从`驱动表`中产生的结果集，分别需要到`t2`表中查询记录，因为是根据`t1`表中的结果去查询`t2`，所以`t2`也称为`被驱动表`。

> 被驱动表可能会被访问多次，应避免大量数据带来io性能差，以及避免全表扫描。