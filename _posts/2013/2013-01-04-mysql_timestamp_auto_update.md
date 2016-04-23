---
layout: post
title: "mysql timestamp 类型自动更新"
date: 2013-01-04
author: scsidisk
categories: Database
tags: MySQL, Timestamp
---

mysql创建表时，如果使用timestamp类型没有指定默认值，它会把第一个使用timestamp的字段默认值设定为CURRENT\_TIMESTAMP，如果后面还有其他字段使用timestamp，则指定为'0000-00-00 00:00:00'，同时Extra列中看到on update CURRENT\_TIMESTAMP，注意它会在更新操作时会把该字段时间设置为当前时间，即使你更新时没有指定要更新该字段。例子如下：

```mysql
create table netingcn(
	id int(11),
	ts1 timestamp,
	ts2 timestamp
);
desc netingcn;
+-------+-----------+------+-----+---------------------+-----------------------------+
| Field | Type | Null | Key | Default | Extra |
+-------+-----------+------+-----+---------------------+-----------------------------+
| id | int(11) | YES | | NULL | |
| ts1 | timestamp | NO | | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
| ts2 | timestamp | NO | | 0000-00-00 00:00:00 | |
+-------+-----------+------+-----+---------------------+-----------------------------+

insert into netingcn(id,ts2) values(1,now());
select * from netingcn;
+------+---------------------+---------------------+
| id | ts1 | ts2 |
+------+---------------------+---------------------+
| 1 | 2012-08-04 22:15:20 | 2012-08-04 22:15:20 |
+------+---------------------+---------------------+

update netingcn set ts2 = '2012-08-01' where id = 1;
select * from netingcn;
+------+---------------------+---------------------+
| id | ts1 | ts2 |
+------+---------------------+---------------------+
| 1 | 2012-08-04 22:15:27 | 2012-08-01 00:00:00 |
+------+---------------------+---------------------+
```

在上述的例子中插入值时没有ts1，故使用当前的系统时间默认值。当使用更新，此处只是更新了ts2字段，但ts1字段值也发生了变化。所以需要注意extra信息中的提示"on update CURRENT\_TIMESTAMP"，如果你想使用timestamp类型又不想让该字段存储的值随着更新而自动更新，那就需要创建表是为该类型的字段显示的指定一个默认值，可以使用CURRENT\_TIMESTAMP或0000-00-00 00:00:00。例如：

```mysql
create table netingcn_1(
	ts1 timestamp default CURRENT_TIMESTAMP,
	ts2 timestamp default '0000-00-00 00:00:00'
);
desc netingcn_1;
+-------+-----------+------+-----+---------------------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+-----------+------+-----+---------------------+-------+
| ts1 | timestamp | NO | | CURRENT_TIMESTAMP | |
| ts2 | timestamp | NO | | 0000-00-00 00:00:00 | |
+-------+-----------+------+-----+---------------------+-------+
```

在CREATE TABLE语句中，可以用下面的任何一种方式声明第1个TIMESTAMP列：

- 用DEFAULT CURRENT\_TIMESTAMP和ON UPDATE CURRENT\_TIMESTAMP子句，列为默认值使用当前的时间戳，并且自动更新。
- 不使用DEFAULT或ON UPDATE子句，与DEFAULT CURRENT\_TIMESTAMP ON UPDATECURRENT_TIMESTAMP相同。
- 用DEFAULT CURRENT\_TIMESTAMP子句不用ON UPDATE子句，列为默认值使用当前的时间戳但是不自动更新。
- 不用DEFAULT子句但用ON UPDATE CURRENT\_TIMESTAMP子句，列有默认值0并自动更新。
- 用常量DEFAULT值，列有给出的 默认值。如果列有一个ON UPDATE CURRENT\_TIMESTAMP子句，它自动更新，否则不。

换句话说，你可以为初始值和自动更新的值使用当前的时间戳，或者其中一个使用，或者两个皆不使用。(例如，你可以指定ON UPDATE来启用自动更新而不让列自动初始化）。

在DEFAULT和ON UPDATE子句中可以使用CURRENT\_TIMESTAMP、CURRENT\_TIMESTAMP()或者NOW()。它们均具有相同的效果。
两个属性的顺序并不重要。如果一个TIMESTAMP列同时指定了DEFAULT和ON UPDATE，任何一个可以在另一个的前面。
例子，下面这些语句是等效的：

```mysql
CREATE TABLE t (ts TIMESTAMP);
CREATE TABLE t (ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP);
CREATE TABLE t (ts TIMESTAMP ON UPDATE CURRENT_TIMESTAMP DEFAULT CURRENT_TIMESTAMP);
```


要为TIMESTAMP列而不是第1列指定自动默认或更新，必须通过将第1个TIMESTAMP列显式分配一个常量DEFAULT值来禁用自动初始化和更新。(例如，DEFAULT 0或DEFAULT'2003-01-01 00:00:00')。然后，对于其它TIMESTAMP列，规则与第1个TIMESTAMP列相同，例外情况是不能忽略DEFAULT和ON UPDATE子句。如果这样做，则不会自动进行初始化或更新。

例如：下面这些语句是等效的：

```mysql
CREATE TABLE t (
    ts1 TIMESTAMP DEFAULT 0,
    ts2 TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP);

CREATE TABLE t (
    ts1 TIMESTAMP DEFAULT 0,
    ts2 TIMESTAMP ON UPDATE CURRENT_TIMESTAMP DEFAULT CURRENT_TIMESTAMP);
```

可以对每个连接设置当前的时区，相关描述参见5.10.8节，"MySQL服务器时区支持"。TIMESTAMP值以UTC格式保存，存储时对当前的时区进行转换，检索时再转换回当前的时区。只要时区设定值为常量，便可以得到保存时的值。如果保存一个TIMESTAMP值，应更改时区然后检索该值，它与你保存的值不同。这是因为在两个方向的转换中没有使用相同的时区。当前的时区可以用作time_zone系统变量的值。

可以在TIMESTAMP列的定义中包括NULL属性以允许列包含NULL值。例如：

```mysql
CREATE TABLE t(
    ts1 TIMESTAMP NULL DEFAULT NULL,
    ts2 TIMESTAMP NULL DEFAULT 0,
    ts3 TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP
);
```

如果未指定NULL属性，将列设置为NULL设置则会将它设置为当前的时间戳。请注意允许NULL值的TIMESTAMP列不会采用当前的时间戳，除非要么其 默认值定义为CURRENT\_TIMESTAMP，或者NOW()或CURRENT\_TIMESTAMP被插入到该列内。换句话说，只有使用如下定义创建，定义为 NULL的TIMESTAMP列才会自动更新：

```mysql
CREATE TABLE t (ts NULLDEFAULT CURRENT_TIMESTAMP)；
```

否则-也就是说，如果使用NULL而不是DEFAULT TIMESTAMP来定义TIMESTAMP列，如下所示

```mysql
CREATE TABLE t1 (ts NULL DEFAULT NULL);CREATE TABLE t2 (ts NULL DEFAULT '0000-00-00 00:00:00');
```

则必须显式插入一个对应当前日期和时间的值。例如：

```mysql
INSERT INTO t1 VALUES (NOW());INSERT INTO t2 VALUES (CURRENT_TIMESTAMP);
```