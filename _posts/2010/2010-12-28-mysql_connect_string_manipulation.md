---
layout: post
title: "mysql中连接字符串操作"
date: 2010-12-28 14:54
author: scsidisk
categories: Linux
tags: MySQL
Slug: mysql_connect_string_manipulation
---

CONCAT(str1,str2,...)

返回来自于参数连结的字符串。如果任何参数是NULL，返回NULL。可以有超过2个的参数。一个数字参数被变换为等价的字符串形式。
mysql\> select CONCAT('My', 'S', 'QL');
-\> 'MySQL'
mysql\> select CONCAT('My', NULL, 'QL');
-\> NULL
mysql\> select CONCAT(14.3);
-\> '14.3'
如：update test set ID=concat(ID,'ABC');
放在前面就连接到前面，放在后面就连接在后面。
