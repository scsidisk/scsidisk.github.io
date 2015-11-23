---
layout: post
title: 使ssh不用输入密码
date: 2010-12-28 15:22
author: scsidisk
category: Linux
tags: [CentOS, Linux, Shell]
Slug: the_use_of_ssh_do_not_have_to_enter_the_password
---

<div class="t_msgfontfix">
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="t_msgfont" id="postmessage_2275637">
有些时候，我们在复制/移动<span class="t_tag">文件</span>到另一台机器时会用到scp，因为它比较安全。但如果每次都要输入密码，就比较烦了，尤其是在script里。不过，ssh有另一种用密钥对来验证的方

</p>
式。下面写出我生成密匙对的过程，供大家参考。

第一步：生成密匙对，我用的是rsa的密钥。使用命令 "ssh-keygen -t rsa"

<div class="blockcode">
<div id="code0">
1.  [user1@rh user1]\$ ssh-keygen -t rsa
2.  Generating public/private rsa key pair.
3.  Enter file in which to save the key (/home/user1/.ssh/id\_rsa):
4.  Created directory '/home/user1/.ssh'.
5.  Enter passphrase (empty for no passphrase):
6.  Enter same passphrase again:
7.  Your identification has been saved in /home/user1/.ssh/id\_rsa.
8.  Your public key has been saved in /home/user1/.ssh/id\_rsa.pub.
9.  The key fingerprint is:
10. e0:f0:3b:d3:0a:3d:da:42:01:6a:61:2f:6c:a0:c6:e7 user1@rh.test.com
11. [user1@rh user1]\$

</div>
*复制代码*

</div>
生成的过程中提示输入密钥对保存位置，直接回车，接受默认值就行了。接着会提示输入一

个不同于你的password的密码，直接回车，让它空着。当然，也可以输入一个。(我比较懒

，不想每次都要输入密码。) 这样，密钥对就生成完了。

其中公共密钥保存在 \~/.ssh/id\_rsa.pub
私有密钥保存在 \~/.ssh/id\_rsa

然后改一下 .ssh 目录的权限，使用命令 "chmod 755 \~/.ssh"

<div class="blockcode">
<div id="code1">
1.  [user1@rh user1]\$ chmod 755 \~/.ssh
2.  [user1@rh user1]\$

</div>
*复制代码*

</div>
之后把这个密钥对中的公共密钥复制到你要访问的机器上去，并保存为

\~/.ssh/authorized\_keys.

<div class="blockcode">
<div id="code2">
1.  [user1@rh user1]\$ scp \~/.ssh/id\_rsa.pub
    rh1:/home/user1/.ssh/authorized\_keys
2.  user1@rh1's password:
3.  id\_rsa.pub 100% 228 3.2MB/s 00:00
4.  [user1@rh user1]\$

</div>
*复制代码*

</div>
之这样就大功告成了。之后你再用ssh scp sftp
之类的访问那台机器时，就不用输入密码

<p>
了，用在script上更是方便。

</td>
</tr>
</tbody>
</table>
 

</div>
<div id="post_rate_div_2275637">
</div>

