---
layout: post
title: "MySQL Partition（分区）扫盲"
date: 2010-12-28 13:52
author: scsidisk
categories: Linux
tags: MySQL
Slug: mysql_partition_partition_literacy
---

作者：老王

MySQL从5.1.3开始支持Partition，你可以使用如下命令来确认你的版本是否支持Partition：

mysql\> SHOW VARIABLES LIKE '%partition%';

+-------------------+-------+

| Variable\_name | Value |

+-------------------+-------+

| have\_partitioning | YES |

+-------------------+-------+

MySQL支持RANGE，LIST，HASH，KEY分区类型，其中以RANGE最为常用：

CREATE TABLE foo (

id INT NOT NULL AUTO\_INCREMENT,

created DATETIME,

PRIMARY KEY(id, created)

) ENGINE=INNODB PARTITION BY RANGE (TO\_DAYS(created)) (

PARTITION foo\_1 VALUES LESS THAN (TO\_DAYS('2009-01-01')),

PARTITION foo\_2 VALUES LESS THAN (TO\_DAYS('2010-01-01'))

)

即便创建完分区，也可以在后期管理，比如说添加一个新的分区：

ALTER TABLE foo ADD PARTITION (

PARTITION foo\_3 VALUES LESS THAN (TO\_DAYS('2011-01-01'))

)

或者删除一个分区：

ALTER TABLE FOO DROP PARTITION foo\_3;

通过检索information\_schema数据库，能看到我们刚刚创建的分区信息：

SELECT \* FROM PARTITIONS WHERE PARTITION\_NAME IS NOT NULL

此时，打开MySQL的数据目录（SHOW VARIABLES LIKE 'datadir'）：

如果MySQL配置设置了innodb file per
table为ON的话，由于上面定义的是INNODB，则会发现：

foo\#p\#foo\_1.ibd

foo\#p\#foo\_2.ibd

如果创建的是MyISAM表类型的话，则会发现：

foo\#P\#foo\_1.MYD

foo\#P\#foo\_1.MYI

foo\#P\#foo\_2.MYD

foo\#P\#foo\_2.MYI

由此可知通过分区，MySQL会把数据保存到不同的数据文件里，同时索引也是分区的，相对未分区的表来说，分区后单独的数据文件和索引文件的大小都明显降低，效率则明显提升。为了验证这一点，我们做如下实验：

INSERT INTO \`foo\` (\`id\`, \`created\`) VALUES

(1, '2008-01-02 00:00:00'),

(2, '2009-01-02 00:00:00');

然后执行SQL：

EXPLAIN PARTITIONS SELECT \* FROM foo WHERE created = '2008-01-02';

会看到MySQL仅仅在foo\_1分区执行这条查询。理论上效率肯定会快一些，至于具体多少，就看数据量了。实际应用分区的时候，我们还可以通过DATA
DIRECTORY和INDEX
DIRECTORY选项把不同的分区分散到不同的磁盘上，从而进步一提高系统的IO吞吐量。

重要提示：使用分区功能之后，相关查询最好都用EXPLAIN
PARTITIONS过一遍，确认分区是否生效。

到底应该采用哪种分区类型呢？通常来说使用range类型是个不错的选择，不过也不尽然，比如说在主从结构中，主服务器由于很少使用SELECT查询，所以在主服务器上使用range类型的分区通常并没有太大意义，此时使用hash类型的分区相对更好一些，假设使用PARTITION
BY HASH(id) PARTITIONS
10，那么当插入新数据时，会根据id把数据平均分散到各个分区上，由于文件小，所以效率高，更新操作会变得更快。

到底应该按哪个字段来分区呢？通常来说按时间字段分区是个不错的选择，不过还是应该按需求而定，通常有很多种划分应用的方式，比如说按时间，或者按用户，哪种用的多，就选哪种来分区。如果使用主从结构的话，还可能用的更灵活些，有的从服务器使用时间分区，有的从服务器使用用户分区，不过如此一来，当执行查询时，程序里应该负责选择正确的从服务器去查询，写个MySQL
Proxy脚本应该可以透明实现。

分区虽然很爽，但目前的实现还有很多限制：

主键或者唯一索引必须包含分区字段：如PRIMARY KEY(id,
created)，不过对INNODB来说，大主键不爽。

很多时候，使用了分区就不要再使用主键，否则可能影响性能。

只能通过int类型的字段或者返回int类型的表达式来分区：通常使用YEAR或TO\_DAYS等函数。

每个表最多1024个分区：不可能无限制的扩展分区，而且过度使用分区往往会消耗大量系统内存。

采用分区的表不支持外键：相关的约束逻辑必须通过程序来实现。

希望看了上面的简单介绍，大家可以明白应该如何使用分区功能了，不要仅仅把眼光放在Shard等流行技术之上，而忽视了原本使用更简单的Partition，恐龙虽然仅仅是爬行动物，却统治了地球长达千万年，比作为哺乳动物的人类统治地球的时间长得多。

MySQL5.5优化了分区功能，具体信息参考：A deep look at MySQL 5.5
partitioning enhancements。

来源：http://hi.baidu.com/thinkinginlamp/blog/item/2fbd54e79fc3a125b938200b.html
