---
layout: post
title: "Linux下Mail命令收集"
date: 2010-12-28 13:51
author: scsidisk
categories: Linux
tags: CentOS, Linux, Mail
Slug: mail_under_linux_commands_collection
---

下面是 Linux 下 Mail 命令的一些用法。

系统收到邮件都会保存在“/var/spool/mail/[linux用户名]”文件中。在linux中输入mail，就进入了收件箱，并显示二十封邮件列表。

此时命令提示符为"&"

    unread 标记为未读邮件
    h|headers 显示当前的邮件列表
    l|list 显示当前支持的命令列表
    ?|help 显示多个查看邮件列表的命令参数用法
    d 删除当前邮件，指针并下移。 d 1-100 删除第1到100封邮件
    f|from 只显示当前邮件的简易信息。 f num 显示某一个邮件的简易信息
    f|from num 指针移动到某一封邮件
    z 显示刚进行收件箱时的后面二十封邮件列表
    more|p|page 阅读当前指针所在的邮件内容 阅读时，按空格键就是翻页，按回车键就是下移一行
    t|type|more|p|page num 阅读某一封邮件
    n|next|{什么都不填} 阅读当前指针所在的下一封邮件内容 阅读时，按空格键就是翻页，按回车键就是下移一行
    v|visual 当前邮件进入纯文本编辑模式
    n|next|{什么都不填} num 阅读某一封邮件
    top 显示当前指针所在的邮件的邮件头
    file|folder 显示系统邮件所在的文件，以及邮件总数等信息
    x 退出mail命令平台，并不保存之前的操作，比如删除邮件
    q 退出mail命令平台,保存之前的操作，比如删除已用d删除的邮件，已阅读邮件会转存到当前用户家目录下的mbox文件中。如果在mbox中删除文件才会彻底删除。

在linux文本命令平台输入 `mail -f mbox`，就可以看到当前目录下的mbox中的邮件了。

cd 改变当前所在文件夹的位置

写信时，连按两次Ctrl+C键则中断工作，不送此信件。

读信时，按一次Ctrl+C，退出阅读状态。

--------------------------------------

Linux邮件命令用法

1. 将文件当做电子邮件的内容送出
    语法：`mail -s “主题”用户名@地址\< 文件`

    例如：

        mail -s “program” user \< file.c 将file.c

    当做mail的内容，送至user，主题为program。

2. 传送电子邮件给本系统用户

    语法：`mail 用户名`

3. 传送电子邮件至外地用户

    语法： mail 用户名@接受地址 /usr/lib/sendmail -bp “Mail queue is empty” ......mail ....

    例如：

        mailtest@hotmail.com
        Subject : mail test
        :
        键入信文内容
        : :
        按下Ctrl+D 键或. 键结束正文。
        连按两次Ctrl+C键则中断工作，不送此信件。
        Cc( Carbon copy) : 复制一份正文，给其他的收信人。

4. 检查所传送的电子邮件是否送出，或滞留在邮件服务器中

    语法：/usr/lib/sendmail -bp
    若屏幕显示为“Mail queue is empty” 的信息，表示mail 已送出。
    若为其他错误信息，表示电子邮件因故尚未送出。

---------------


Linux下mail使用技巧

系统提供了用户之间通信的邮件系统，当用户打开终端注册登录时发现系统给出如下信息： you have mail.

这时用户可通过键入mail命令读取信件：

    $ mail

mail程序将逐个显示用户的信件，并依照时间顺序，显示最新的信件。每显示一段信件，mail都询问用户是否要对该信件作些处理。若用户回答d，则表示 删除信件；若仅按回车键，表示对信件不作任何改动（信件仍旧保存，下次还可读这一信件）；若回答p，则要求重复显示信件；s filename表示要把信件存入所命名的文件；若回答q，表示要从mail退出。

我们在本章的第一个例子中演示了如何写一封信，作为练习，你可送信件给自己，然后键入mail读取自己发的信件，看看会有什么效果。（发信给自己是一种设置备忘录的方法）。

    $mail frank 给自己写信
    subject: test
    This is a mail test
    CRL-d
    EOT
    $
    $mail 查看信件
    “/var/spool/mail/frank:”1 message 1 new
    \>Nfrank@xteam.xteamlinux.comThu Mar 25 11:00 13/403 “test”
    &
    Message 1:
    From frank Thu Mar 25 11:00:25 1999/3/25
    Received: (fromfrank@localhost)
    by xteam.xteamlinux.com(8.8.4/8.8.4)
    id LAA05170 for frank;Thu 25 Mar 1999 11:00:25 GMT
    date: Thu,25 Mar 1999 11:00:25 GMT
    From:RHS Linux User
    Message-Id:\<199903251142.LAA05170@xteam.xteamlinux.com\>
    To:frank@xteam.xteamlinux.com
    Subject:test
    Status:R
    This is a mail test
    &

mail命令还有很多其它用法，例如发送事先准备好的信件，或一次送信给若干人。还可以用其它方法送信件。


[From [<cite>Linux下Mail命令收集 - v -
博客园</cite>](http://www.cnblogs.com/vinyfeng/archive/2010/01/04/1639059.html)]

