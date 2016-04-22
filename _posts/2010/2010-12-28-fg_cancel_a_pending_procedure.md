---
layout: post
title: "fg：取消挂起程序"
date: 2010-12-28 13:52
author: scsidisk
category: Linux
tags: [CentOS, Linux, Shell, 命令]
Slug: fg_cancel_a_pending_procedure
---

作用：fg命令使一个被挂起的进程在前台执行。

用法：fg [job-spec]

[job-spec]：后台任务号码。

说明：fg命令和bg命令是相对应的。如果想查看后台程序运行情况，可以使用fg命令把它调回前台查看。bg命令可以使多个进程放到后台中执行。

应用实例如下。

使用fg命令时，要加入后台任务号码，如果不加任何号码，则所变动的均是当前任务。

＃du -a / | sort -rn \> /tmp/du.sorted &

[1] 237

\#fg 1

<div class="posttagsblock">
</div>

