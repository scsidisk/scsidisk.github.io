---
layout: post
title: "shell参数"
date: 2010-12-28 15:22
author: scsidisk
categories: Linux
tags: CentOS, Linux, Shell
Slug: 2010-12-28-shell_avgs
---

<span style="font-family: 'Lucida Grande', »ªÎÄÏ¸ºÚ, STHeiti; line-height: 18px;">在shell中，表示值是用\$,相当于DOS中的%。</span>

<div class="blog_content">
<span style="font-family: 'Lucida Grande', »ªÎÄÏ¸ºÚ, STHeiti; line-height: 18px;"><span style="font-family: 'Lucida Grande', »ªÎÄÏ¸ºÚ, STHeiti; line-height: 18px;">
1.位置参数
一般是系统或用户提供的参数。
\$[0-n],\$0,表示指令本身，\$1表示第一个参数，一次类推。
\$0是内部参数，必须要有的，其后的就可有可无了</span></span>2.内部参数
\$\# ----参数数目
\$?
----上一个代码或者shell程序在shell中退出的情况，如果正常退出则返回0，反之为非0值。
\$\* ----所有参数的字符串

</p>
</div>
<div class="posttagsblock">
[ ](http://technorati.com/tag/CentOS)

</div>

