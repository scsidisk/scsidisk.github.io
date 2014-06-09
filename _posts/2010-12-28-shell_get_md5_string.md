---
layout: post
title: shell下取得字符串的md5值
date: 2010-12-28 15:22
author: scsidisk
category: CentOS
tags: CentOS, Linux, md5, md5sum, PHP, Shell
Slug: 2010-12-28-shell_get_md5_string
---

shell下取得字符串的md5值

echo 123|md5sum

ba1f2511fc30423bdbb183fe33f3dd0f -

php -r "echo md5('123');"

202cb962ac59075b964b07152d234b70

两者的md5值不一致，于是有很多有趣的解法：

1 Mysql解法：

mysql\> select md5('123');

+----------------------------------+

| md5('123') |

+----------------------------------+

| 202cb962ac59075b964b07152d234b70 |

+----------------------------------+

2 修正换行符法

[root@fetion \~]\# printf 123|md5sum

202cb962ac59075b964b07152d234b70 -

[root@fetion \~]\# echo -n 123|md5sum

202cb962ac59075b964b07152d234b70 -

[root@fetion \~]\# echo 123|tr -d '/n'|md5sum

202cb962ac59075b964b07152d234b70 -

小结一下：

1 echo默认是带换行符做结尾的

2 echo -n 可以去掉换行符

3 printf是没有换行符结尾的

4 tr可以删掉一个字符，如 tr -d '/n'

5 php命令行执行一段程序是 php -r "code"

<div class="posttagsblock">
[ ](http://technorati.com/tag/CentOS)

</div>

