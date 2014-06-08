Title: nohup：用户退出系统之后继续工作
Date: 2010-12-28 14:54
Author: scsidisk
Category: CentOS
Tags: CentOS, Shell, 命令
Slug: nohup_continue_working_after_the_user_exits_the_system

作用：nohup命令确保执行程序能在用户退出系统之后继续工作。

用法：nohup命令

说明：一般退出Linux系统时，会把所有的程序全部结束掉，包括那些后台程序。但有时候，例如，用户正在下载一个很大的文件，但是因下班或有事需要先退出系统，希望退出系统时程序还能继续执行。这时，我们就可以使用nohup命令使进程在用户退出后仍继续执行。同时这些进程都在后台执行（命令放到后台运行，nohup必须与&操作同时使用），结果则会写到用户自己的目录下的nohup.out文件里。

应用实例如下。

程序在后台自动执行：

nohup wget -c -t0 http://www.bsdr.com/ghs1.rar &

<div class="posttagsblock">
</div>

