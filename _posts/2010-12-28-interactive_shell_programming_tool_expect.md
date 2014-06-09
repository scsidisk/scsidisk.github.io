---
layout: post
title: 交互式shell编程利器expect
date: 2010-12-28 15:22
author: scsidisk
category: CentOS
tags: CentOS, Linux, Shell
Slug: interactive_shell_programming_tool_expect
---

<div class="post-content">
手里有几台Linux服务器需要经常添加用户，每次都要登录到相应的机器上去添加，特别麻烦。于是想，可不可以在一台机器上写一个脚本来远程管理其它服务器呢？

目标首先瞄准了我熟悉的PHP-CLI，它有一个开发中的模块ssh2，可以完成相应的功能。这个不想说了，因为用了半天都不行，Bug还太多，建议大家如非必要还是不要用这个模块的好。

没了PHP，很迷茫，然后很幸运地发现了expect。expect是交互式shell编程的利器，可以根据返回值来确定下面发送什么命令，特别好用。我把自己编写的远程增加用户的shell跟大家分享下（需要机器装有expect，没有的自己装吧），脚本如下：  
<span id="more-1505"></span>

~~~~ {lang="bash" xml:lang="bash"}
#!/usr/bin/expect
#脚本第一个参数是远程服务器IP
set IP     [lindex $argv 0]
#远程服务器用户名（通常用root）
set USER [lindex $argv 1]
#远程服务器用户名的密码
set PASSWD [lindex $argv 2]
#添加的新用户
set Nuser [lindex $argv 3]
#新用户的密码
set Npasswd [lindex $argv 4]
#用spawn启动一个ssh客户端
spawn ssh -l $USER $IP
#如果是第一次连接，要保存密钥再输入密码，如果不是第一次连接则输入密码
expect {
"yes/no" { send "yes/r"; exp_continue }
"password:" { send "$PASSWD/r" }
}
#如果不是root，要expect "$"，下面不讲了，很简单
expect "*#"
send "useradd -s /bin/sh -d /home/$Nuser $Nuser/r"
expect "*#"
send "passwd $Nuser/r"
expect "*password:"
send "$Npasswd/r"
expect "*password:"
send "$Npasswd/r"
expect "*#"
send "exit/r"
~~~~

 

</div>

