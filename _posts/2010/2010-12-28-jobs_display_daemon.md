---
layout: post
title: "jobs：显示后台程序"
date: 2010-12-28 13:53
author: scsidisk
category: Linux
tags: [CentOS, Linux, Shell, 命令]
Slug: jobs_display_daemon
---

作用：jobs命令显示后台任务的执行情况。

用法：jobs [选项] [jobspec…]

主要选项如下。

-l：长输出用法，显示全部内容。

-n：不输出信息。

-p：只输出进程号。

-r：只输出运行的进程。

[jobspec]表示后台任务号码。

应用实例如下。

先把两个进程放在系统后台运行，然后使用jobs命令查看后台任务的执行情况：

＃du -a /etc \> user.data &

[1] 233

\# find / -name core -type f -ls \> core.data &

[2] 234

\#jobs -l

[1] + 237 Running du -a /etc \> user.data

[2] - 238 Running find / -name core -type f -ls \> core.data

说明：

上面的当前任务是"du -a /etc \>
user.data"，因为后台任务号码是"[1]"。当第一个后台任务顺利执行完毕，第二个后台任务还在执行中时，当前任务便会自动变成后台任务号码"[2]"的后台任务，即当前任务是动态的。

<div class="posttagsblock">
</div>

