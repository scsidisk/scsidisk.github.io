---
layout: post
title: "Mac OS X 上发送 GPG 加密邮件"
date: 2016-06-04
author: scsidisk
categories: MacOSX
tags: MacOSX, Mail, GPG
---

研究比特币的人一定都听说过 PGP 加密邮件通讯。传说中本聪和小伙伴们发邮件都是要用 PGP 加密的。

1991年，程序员 Phil Zimmermann 为了避开“机构”监视，开发了加密软件 PGP 。这个软件非常好用，迅速流传开来，成了许多程序员的必备工具。但是，它是商业软件，不能自由使用。所以，自由软件基金会决定，开发一个 PGP 的替代品，取名为 GnuPG 。这就是 GPG 的由来。

GPG 有许多用途，本文主要介绍 Mac OS X 下面的邮件加密，不同的邮件客户端有不同的设置，参见下面的链接。

- OS X    https://ssd.eff.org/en/module/how-use-pgp-mac-os-x
- Linux   https://ssd.eff.org/en/module/how-use-pgp-linux
- Windows https://ssd.eff.org/en/module/how-use-pgp-windows-pc
- iOS     https://itunes.apple.com/app/ipgmail/id430780873?mt=8
- Android https://play.google.com/store/apps/details?id=org.sufficientlysecure.keychain

### 安装

gpgtools.org 上已经提供了集成的工具包来进行GPG的加密等相关工作。

首先从 http://www.gpgtools.org/ 下载 gpgtools，下载后进行安装。工具包中包括了如下软件：

GPGMail、GPG Keychain Access、MacGPG1、MacGPG2、GPGService、GPGPreference、Mobile OpenGPG。

安装之后，你就可以在 terminal 中看到 gpg 命令已经被安装好了。

brew cask 也可以安装，不过有的机器不能正常使用，暂时没有找到解决办法。

### 生成密钥

打开 GPG Keychain Access 生成自己的公钥和密钥对。

正常情况下，第一次打开的时候，列表中应该是个空白的界面，紧接着会提醒用户生成自己的密钥对。

![](/static/images/2015/06/gpg_mail_mac_1.png)

创建密钥对，默认会用你的电脑帐号对应的邮箱，你也可以选择或者输入其他的邮箱。Full name 部分注意用户名要大于五个字符，所以中文姓名会有提示。Length  一般选择 2048 , 也可以选择 4096 这样安全性能够更高一些。

输入一个自己可以记住的密码，没有密码也可以生成，只是感觉不安全。

勾选上传公钥，可以把生成后的公钥，上传到服务器，方便别人查找。

完成后，点击 Generate Key，生成自己的密钥对。

### 导入公钥

可以从公钥服务器上面查找收件人的公钥，或向对方要到公钥。

搜索的时候可以是用户名，完整的邮箱都可以，如果有重复的，查看注释选择正确的收件人邮件即可。

导入公钥以后可以发送加密邮件。

### 加密邮件

打开 Mail 程序，新建邮件，可以在右上角看到 OpenGPG 的标识，点击可以选择 S/MIME 两种加密方式。

S/MIME 是另外一种邮件加密方式，需要申请证书，参见 [Mac 上面使用免费的电子邮件证书](http://scsidisk.github.io/2014/08/17/mac_mail_comodo_certificate/) 。

![](/static/images/2015/06/gpg_mail_mac_2.png)

签名上面的两个按钮，前面是加密，后面是签名。

如果你有收件人的公钥可以加密邮件，否则只能对邮件进行签名。

### 查看加密邮件

如果对方使用你的公钥加密的邮件，你可以 Mail 中直接看到邮件的内容。

### 使用不支持 GPG 客户端查看

如果使用类似 gmail 等不支持 GPG 的客户端中查看，可以使用文本编辑器打开加密部分(附件)，全选加密内容，选择软件菜单(左上角你使用的软件名称)，点击服务，选择 OpenGPG：Encrypt Selection，即可看到解密后的内容。

也可以把对应操作设置成快捷键：

进入“系统设置”-->“键盘”-->“快捷方式”，鼠标点击左边的“服务”选项，在右侧中找到一系列以“OpenPGP开头”的操作，把前面的勾去掉。

接着为以下四个操作设置快捷键，打勾启用：

- OpenPGP: Decrypt设置为：control+option+command+minus（-）
- OpenPGP: Encrypt设置为：control+option+command+equals（=）
- OpenPGP: Sign设置为：control+option+command+open bracket（[）
- OpenPGP: Verify设置为：control+option+command+close bracket（]）

### GPG 命令行

安装 gpgtools 以后就安装上了 gpg 命令行。下面是部分命令。

更多命令参考 <http://www.ruanyifeng.com/blog/2013/07/gpg.html>

    # 列出密钥
    gpg --list-keys
    # 加密文件
    gpg --recipient [用户ID] --output demo.en.txt --encrypt demo.txt
    # 解密文件
    gpg --decrypt demo.en.txt --output demo.de.txt
    gpg demo.en.txt
    # 签名文件
    gpg --sign demo.txt
    # 验证签名
    gpg --verify demo.txt.asc demo.txt


### 参考

- <https://www.ezloo.com/2015/01/pgp_for_mac_os_x.html>
- <http://www.cnblogs.com/cocowool/archive/2011/11/14/2249004.html>
- <http://www.ruanyifeng.com/blog/2013/07/gpg.html>
- <http://www.ruanyifeng.com/blog/2013/07/gpg.html>
- <https://songchenwen.com/tech/2015/05/20/pgp-mail-on-osx/>





