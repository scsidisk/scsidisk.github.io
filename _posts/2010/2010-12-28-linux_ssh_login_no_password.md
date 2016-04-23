---
layout: post
title: "linux ssh 无密码登陆"
date: 2010-12-28 14:56
author: scsidisk
categories: Linux
tags: CentOS, Linux, SSH
Slug: 2010-12-28-linux_ssh_login_no_password
---

ssh 无需密码登陆

ssh-keygen -t rsa 会创建\~/.ssh/id-ras.pub id-ras

公钥：/root/.ssh/id-ras.pub

私钥：/root/.ssh/id-ras

chmod 755 /root/.ssh (可不做)

把公钥复制到需要访问的机器上 /.ssh/ 并改名保存为 authorized\_keys
,如果是多台机器需要,无密码登陆,则各自机器产生公钥追加到authorized\_keys即可.

假设服务器IP地址为192.168.1.1，机器名：cluster.hpc.org

客户端IP地址为172.16.16.1，机器名：p470-2.wangrx.sioc.ac.cn

客户端用户yzhao需要使用ssh无密码登录服务器的zhaoy帐户

实现原理

使用一种被称为"公私钥"认证的方式来进行ssh登录.
"公私钥"认证方式简单的解释是

首先在客户端上创建一对公私钥 （公钥文件：\~/.ssh/id\_rsa.pub；
私钥文件：\~/.ssh/id\_rsa）

然后把公钥放到服务器上（\~/.ssh/authorized\_keys）, 自己保留好私钥

当ssh登录时,ssh程序会发送私钥去和服务器上的公钥做匹配.如果匹配成功就可以登录了

设置如下：

1、以yzhao用户登录客户机器并在客户端机器上执行"ssh-keygen -t rsa"

(注：每次执行"ssh-keygen -t rsa"产生的私钥文件都会不同)

a）如果文件"\~/.ssh/id\_rsa"存在，会提示是否覆盖该文件，此时可选择"n"不覆盖该文件而使用已有的id\_rsa文件；如果选择"y"则会重新生成"\~/.ssh/id\_rsa"文件，接下来会提示输入passphrase，回车确定使用空的passphrase，再次回车确认（这里也可以输出passphrase，相当于ssh时登录的密码）。然后会重新生成"\~/.ssh/id\_rsa"文件和"\~/.ssh/id\_rsa.pub"文件（结果如下）。

[yzhao@p470-2 \~]\$ ssh-keygen -t rsa

Generating public/private rsa key pair.

Enter file in which to save the key (/disk2/yzhao/.ssh/id\_rsa):

/disk2/yzhao/.ssh/id\_rsa already exists.

Overwrite (y/n)? y

Enter passphrase (empty for no passphrase):

Enter same passphrase again:

Your identification has been saved in /disk2/yzhao/.ssh/id\_rsa.

Your public key has been saved in /disk2/yzhao/.ssh/id\_rsa.pub.

The key fingerprint is:

6d:a1:17:8a:b6:d2:c0:a1:6c:66:ba:85:0b:7b:9f:0c
yzhao@p470-2.wangrx.sioc.ac.cn

b）如果"\~/.ssh/id\_rsa"文件和"\~/.ssh/id\_rsa.pub"文件不存在则会自动创建新的"\~/.ssh/id\_rsa"文件和"\~/.ssh/id\_rsa.pub"文件，passphrase设置同上。

[yzhao@p470-2 \~]\$ ssh-keygen -t rsa

Generating public/private rsa key pair.

Enter file in which to save the key (/disk2/yzhao/.ssh/id\_rsa):

Created directory '/disk2/yzhao/.ssh'.

Enter passphrase (empty for no passphrase):

Enter same passphrase again:

Your identification has been saved in /disk2/yzhao/.ssh/id\_rsa.

Your public key has been saved in /disk2/yzhao/.ssh/id\_rsa.pub.

The key fingerprint is:

54:49:ad:33:b3:ff:71:da:6d:db:78:d0:bb:6a:15:bc
yzhao@p470-2.wangrx.sioc.ac.cn

2、使用ssh
zhaoy@192.168.1.1登录到服务器，编辑服务器上"\~/.ssh/authorized\_keys"文件，将客户端机器上的"\~/.ssh/id\_rsa.pub"文件内容追加到"\~/.ssh/authorized\_keys"文件中。

（注：可以在客户端机器上使用以下命令来实现：

cat \~/.ssh/id\_rsa.pub | ssh zhaoy@192.168.1.1 "cat - \>\>
\~/.ssh/authorized\_keys"

此时会要求输入zhaoy在服务器上的登录密码，输入后即会将客户端机器上的"\~/.ssh/id\_rsa.pub"文件内容追加到服务器上的"\~/.ssh/authorized\_keys"文件中）

如果是首次连接服务器会出现以下的提示，确认连接并输入密码后其他直接回车确定。

[yzhao@p470-2 \~]\$ ssh zhaoy@192.168.1.1

The authenticity of host '192.168.1.1 (192.168.1.1)' can't be
established.

RSA key fingerprint is 94:91:33:01:6b:e7:10:ae:42:ac:ea:5c:8c:bb:f1:18.

Are you sure you want to continue connecting (yes/no)? yes

Warning: Permanently added '192.168.1.1' (RSA) to the list of known
hosts.

zhaoy@192.168.1.1's password:

Last login: Fri Dec 21 17:41:38 2007 from 172.16.16.1

Rocks 4.2.1 (Cydonia)

Profile built 03:58 21-Jun-2007

Kickstarted 12:25 21-Jun-2007

Rocks Frontend Node - Our Cluster Cluster

It doesn't appear that you have set up your ssh key.

This process will make the files:

/home/zhaoy/.ssh/id\_rsa.pub

/home/zhaoy/.ssh/id\_rsa

/home/zhaoy/.ssh/authorized\_keys

Generating public/private rsa key pair.

Enter file in which to save the key (/home/zhaoy/.ssh/id\_rsa):

Created directory '/home/zhaoy/.ssh'.

Enter passphrase (empty for no passphrase):

Enter same passphrase again:

Your identification has been saved in /home/zhaoy/.ssh/id\_rsa.

Your public key has been saved in /home/zhaoy/.ssh/id\_rsa.pub.

The key fingerprint is:

7e:f6:ab:b0:79:70:cb:c9:f7:40:37:aa:10:4d:4a:ac zhaoy@cluster.hpc.org

3、如果在第1步中使用了空的passphrase，则可以跳过第4步，此时在客户端即可以使用"ssh
zhaoy@192.168.1.1"即可无密码登录到服务器；如果第一步中设置了passphrase，则继续执行以下步骤。

4、如果第1步中设置了passphrase，则此时需要输入该passphrase登录服务器。此时前面我们把输入密码变成了输入passphrase，这没有带来任何方便。但是我们可以通过ssh-agent来帮助我们自动输入passphrase(只是看起来像是自动输入而已)，我们只要在第一次登录时输入一次passphrase,
以后的工作就可以交给ssh-agent。在客户端机器上执行命令ssh-add，这里会提示输入一次passphrase。输入第一步中设置的passphrase之后会修改"\~/.ssh/id\_rsa"文件。再在客户端执行"ssh
zhaoy@192.168.1.1"即可无密码登录到服务器端。

[yzhao@p470-2 \~]\$ ssh-add

Enter passphrase for /disk2/yzhao/.ssh/id\_rsa:

Identity added: /disk2/yzhao/.ssh/id\_rsa (/disk2/yzhao/.ssh/id\_rsa)

[yzhao@p470-2 \~]\$ ssh zhaoy@192.168.1.1

Last login: Fri Dec 21 17:55:38 2007 from 172.16.16.1

Rocks 4.2.1 (Cydonia)

Profile built 03:58 21-Jun-2007

Kickstarted 12:25 21-Jun-2007

Rocks Frontend Node - Our Cluster Cluster

[zhaoy@cluster \~]\$

<div class="posttagsblock">
</div>

