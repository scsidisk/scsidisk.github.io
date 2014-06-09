---
layout: post
title: 高性能MySQL读书笔记：找出谁持有锁
date: 2010-12-28 13:52
author: scsidisk
category: MySQL
tags: MySQL, 调优
Slug: high_performance_mysql_reading_notes_find_out_who_holds_the_lock
---

作者：老王

周末重读了一遍《高性能MySQL》，发现有些知识点看过便忘了，没有实际动手操作一遍就是记不牢，所以今天动手操作了一下“找出谁持有锁”，并把实验步骤记录下来，有兴趣的网友可以参照一二。

问题的背景：在实际使用MySQL时，如果访问量比较大，那么很可能会出现大量Locked状态的进程，但是却不能方便的识别是哪条SQL引起的问题，很多人遇到此类问题时，多半是通过PhpMyAdmin查询可疑SQL，然后KILL掉，但问题是可疑SQL可能会很多，这样逐一尝试太过笨拙，有的人一怒之下很可能会重启MySQL，但如此治标不治本的方法肯定更不可取。

开始实验，在test数据库先建立一个测试表foo（注意：是MyISAM表类型），添加若干数据：

CREATE TABLE IF NOT EXISTS \`foo\` (

\`id\` int(10) unsigned NOT NULL AUTO\_INCREMENT,

\`str\` varchar(100) NOT NULL,

PRIMARY KEY (\`id\`)

) ENGINE=MyISAM;

INSERT INTO \`foo\` (\`id\`, \`str\`) VALUES

(1, 'a'),

(2, 'b');

打开一个MySQL命令行终端：

mysql\> USE test;

mysql\> SELECT SLEEP(12345) FROM foo;

再打开一个MySQL命令行终端：

mysql\> USE test;

mysql\> UPDATE foo SET str='bar';

此时执行SHOW PROCESSLIST，可以看到已经出现Locked现象了：

10 User sleep SELECT sleep(12345) FROM foo

20 Locked UPDATE foo SET str = 'bar'

当然，我们知道是SLEEP堵塞了UPDATE，但如果不是这个实验，面对同样的情况，比如说几百个SQL查询同时映入眼帘，我们如何来判断呢？此时没人能打包票，只能瞎蒙了，经验有时候很重要，但我们还需要明确的命令，在这里就是：

mysqladmin debug

注意：如何你没有设定“.my.cnf”配置文件的话，可能需要输入用户名和密码参数

命令执行后，不会有任何明确的输出，不要着急，有价值的东西此时已经被保存到了错误日志里：

mysql\> SHOW VARIABLES LIKE 'log\_error';

找到错误日志的具体路径后，打开，查看日志的最后部分：

10 test.foo Locked - read Low priority read lock

20 test.foo Waiting - write High priority write lock

如此，我们就能看到id是10的SQL堵塞了id是20的SQL，至于具体的SQL，到SHOW
PROCESSLIST里对照一下就能看到了。

来源：http://hi.baidu.com/thinkinginlamp/blog/item/d6daa61e9f9acc11413417cc.html

<div class="posttagsblock">
</div>

