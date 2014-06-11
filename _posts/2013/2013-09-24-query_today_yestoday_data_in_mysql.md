---
layout: post
title: mysql查询今天、昨天、7天、近30天、本月、上一月 数据
date: 2013-10-20 11:02
author: scsidisk
category: MySQL
tags: MySQL
---

今天

```mysql
select * from '表名' 
where to_days('时间字段名') = to_days(now());
```

昨天

```mysql
SELECT * FROM '表名' 
WHERE TO_DAYS( NOW( ) ) - TO_DAYS( '时间字段名') <= 1
```

7天

```mysql
SELECT * FROM '表名' 
where DATE_SUB(CURDATE(), INTERVAL 7 DAY) <= date('时间字段名')
```

近30天

```mysql
SELECT * FROM '表名' 
where DATE_SUB(CURDATE(), INTERVAL 30 DAY) <= date('时间字段名')
```

本月

```mysql
SELECT * FROM '表名' 
WHERE DATE_FORMAT( '时间字段名', '%Y%m' ) = DATE_FORMAT( CURDATE( ) , '%Y%m' )
```

上一月

```mysql
SELECT * FROM '表名' 
WHERE PERIOD_DIFF( date_format( now( ) , '%Y%m' ) , date_format( '时间字段名', '%Y%m' ) ) =1
```

同时，再附上 一个 mysql官方的相关document

http://dev.mysql.com/doc/refman/5.1/zh/tutorial.html

查询当前这周的数据

```mysql
SELECT name,submittime FROM enterprise WHERE YEARWEEK(date_format(submittime,'%Y-%m-%d')) = YEARWEEK(now());
```

查询上周的数据

```mysql
SELECT name,submittime FROM enterprise WHERE YEARWEEK(date_format(submittime,'%Y-%m-%d')) = YEARWEEK(now())-1;
```

查询当前月份的数据

```mysql
select name,submittime from enterprise   
where date_format(submittime,'%Y-%m')=date_format(now(),'%Y-%m')
```

查询距离当前现在6个月的数据

```mysql
select name,submittime from enterprise 
where submittime between date_sub(now(),interval 6 month) and now();
```

查询上个月的数据﻿

```mysql
select name,submittime from enterprise   
where date_format(submittime,'%Y-%m')=date_format(DATE_SUB(curdate(), INTERVAL 1 MONTH),'%Y-%m')

select * from `user` where DATE_FORMAT(pudate,'%Y%m') = DATE_FORMAT(CURDATE(),'%Y%m') ;

select * from user where WEEKOFYEAR(FROM_UNIXTIME(pudate,'%y-%m-%d')) = WEEKOFYEAR(now())

select *
from user
where MONTH(FROM_UNIXTIME(pudate,'%y-%m-%d')) = MONTH(now())

select *
from [user]
where YEAR(FROM_UNIXTIME(pudate,'%y-%m-%d')) = YEAR(now())
and MONTH(FROM_UNIXTIME(pudate,'%y-%m-%d')) = MONTH(now())

select * from [user] where pudate between '上月最后一天' and '下月第一天'
```