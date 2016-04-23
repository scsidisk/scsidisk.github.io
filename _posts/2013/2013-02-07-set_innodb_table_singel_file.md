---
layout: post
title: "(原)让innodb表保存为单独的文件"
date: 2013-02-07
author: scsidisk
categories: Database
tags: MySQL
---

mysql的Innodb引擎生成的表会在同一个文件中，如果想让不同的表保存为单独的文件，可以使用下面的方法。

### 1. my.cnf中添加

```
[mysqld]
innodb_file_per_table = 1
```

### 2. 启动参数添加

如果不起作用(如:MAMP) 可以在启动命令里面添加

```
--innodb_file_per_table=1
```

### 3. 验证;

```
mysql> show variables like '%per_table%';
+-----------------------+-------+
| Variable_name | Value |
+-----------------------+-------+
| innodb_file_per_table | ON |
+-----------------------+-------+
1 rows in set (0.03 sec)
```

### 4. 把现有的表改成单独文件

```mysql
-- Move table from system tablespace to its own tablespace.
SET GLOBAL innodb_file_per_table=1;
ALTER TABLE table_name ENGINE=InnoDB;

-- Move table from its own tablespace to system tablespace.
SET GLOBAL innodb_file_per_table=0;
ALTER TABLE table_name ENGINE=InnoDB;
```