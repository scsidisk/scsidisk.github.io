---
layout: post
title: "Kerberos for Mac OS X"
date: 2014-07-17
author: scsidisk
categories: Java
tags: Java, kerberos
---

在mac上面使用kerberos比较麻烦，mac本身已经安装了kerberos，但不好使，所以在网上查了一下，记录下面安装过程。

### 下载 Kerberos Extras for Mac OS X

[这里有介绍](http://kb.mit.edu/confluence/pages/viewpage.action?pageId=55738387)

在这里[下载](http://web.mit.edu/macdev/www/osx-kerberos-extras.html)并安装

注意，如果想让kerberos好使的前提是你的mac用户名和kerberos用户名必须相同，并且二者的密码也要相同,下面使用方法规避。

### 修改配置文件

配置文件在 /Library/Preferences/edu.mit.Kerberos，而不是 /etc/krb5.conf。

```
#Version 1.0
#First installed with Kerberos Extras 10.5 (a2)

[libdefaults]
    default_realm = ATHENA.MIT.EDU
    noaddresses = TRUE

[realms]
    ATHENA.MIT.EDU = {
        kdc = kerberos.mit.edu.:88
        kdc = kerberos-1.mit.edu.:88
        kdc = kerberos-2.mit.edu.:88
        admin_server = kerberos.mit.edu.
        default_domain = mit.edu
    }

[domain_realm]
    .mit.edu = ATHENA.MIT.EDU
    mit.edu = ATHENA.MIT.EDU

[v4 realms]
    ATHENA.MIT.EDU = {
        kdc = kerberos.mit.edu.
        kdc = kerberos-1.mit.edu.
        kdc = kerberos-2.mit.edu.
        admin_server = kerberos.mit.edu.
        default_domain = mit.edu
        string_to_key_type = mit_string_to_key
    }

[v4 domain_realm]
    .mit.edu = ATHENA.MIT.EDU
    mit.edu = ATHENA.MIT.EDU
```

其中：

- ATHENA.MIT.EDU 改为自己的host
- kerberosxxx.mit.edu 改为自己的server

修改以后大致为：

```
[libdefaults]
    default_realm = XXX.XXX
    noaddresses = TRUE

[realms]
	XXX.XXX = {
	   kdc = krb1.xxx.cn
	   admin_server = krb1.xxx.cn:666
	   kpasswd_server = krb1.xxx.cn:555
	}

[domain_realm]
    .mit.edu = XXX.XXX
    mit.edu = XXX.XXX

[v4 realms]
	XXX.XXX = {
	   kdc = krb1.xxx.cn
	   admin_server = krb1.xxx.cn:666
	   kpasswd_server = krb1.xxx.cn:555
	}

[v4 domain_realm]
    .mit.edu = XXX.XXX
    mit.edu = XXX.XXX
```


### 认证

在命令行中执行

```
$ kinit yourusername@INF.ED.AC.UK
toby@INF.ED.AC.UK's Password:   ## 输入密码
$ klist ## 查看认证
```

- XXX.XXX 改为自己的host
- xxx.cn 改为自己的server

也可以使用  Ticket Viewer （票据显示程序）进行认证

进入  /System/Library/CoreServices 打开 票据显示程序

点击 Add Identity 添加

输入 XXX.XXX 和 密码。 认证成功

### ssh 链接

现在kinit以后，认证成功，但是ssh到服务器后仍然要密码，这说明ssh的时候没有通过认证，猜想可能kerberos的服务没有找到server或者超时，于是通过ssh -vvv来查看log，发现有条信息是与成功登陆的log是不同的==>Miscellaneous failure (see text)，于是上网搜了一下，在stackoverflow中找到了答案，在ssh的配置文件中要将两个选项打开：GSSAPIAuthentication yes；GSSAPITrustDNS yes(ssh配置是/etc/ssh_config)，到此，kerberos认证成功。
