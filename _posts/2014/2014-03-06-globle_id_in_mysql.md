---
layout: post
title: (原)使用mysql实现全局id
date: 2014-03-06
author: scsidisk
category: Database
tags: [MySQL]
---

mysql分表的时候经常需要全局id保证多个表中的数据使用不重复的编号。
下面的方法可以很好的满足要求。

### 1. 创建表

```mysql
CREATE TABLE `tbl_globle_id` (
  `name` char(64) NOT NULL COMMENT '应用名称',
  `value` bigint(20) unsigned NOT NULL DEFAULT '1' COMMENT '统一id',
  PRIMARY KEY (`name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='全局 统一id'
```

### 2. 更新值

```mysql
UPDATE tbl_globle_id SET value=LAST_INSERT_ID(value+1) where name='app1'
```

### 3. 取得自己最后更新的值

```mysql
select LAST_INSERT_ID()
```