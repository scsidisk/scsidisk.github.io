---
layout: post
title: Mac 上面使用免费的电子邮件证书
date: 2014-08-17
author: scsidisk
category: MacOSX
tags: [MacOSX, Mail]
---

现在为了数字安全越来越多人使用数字证书发送Email，那有没有免费的电子邮件数字证书呢？Comodo提供免费的email数字证书,而且comodo是全球第二大证书机构，可以值得信赖！

电子邮件证书提供让您进行数字签名和加密电子邮件和附件的保密性和安全性的电子通信最强水平。 加密意味着只有你预期的收件人将可以阅读数字签名的邮件，同时允许他们将其确定为发件人的电子邮件和验证消息不符合途中被篡改。 我们的电子邮件证书是为个人/家庭用户免费。

- 确保电子邮件是私有的多达256位安全加密
- 您的电子邮件进行数字签名，以确保真实性和完整性
- 值得信任，如Microsoft Outlook，Windows邮件，雷鸟邮件客户端的主要客户端支持
- 简单的在线应用和安装方法，你可以在几分钟内就设置
- 免费供个人使用！ 安全通信是一种权利 – 而不是昂贵的奢侈品

### 1. 申请证书

打开 Comodo Free Email Certificate 申请网址： http://www.comodo.com/home/email-security/free-email-certificate.php  点击 sign up now

或者直接进入安全网址：https://secure.comodo.com/products/frontpage?area=SecureEmailCertificate 进行申请！

### 添加证书

完成后会收到邮件，打开主题为：Your certificate is ready for collection!的电子邮件。下载证书得到 CollectCCC.p7s

双击后弹出签名，输入密码后添加的密钥串("登录")中。

安装证书后最好将证书备份，防止丢失！

### 2. 使用证书

在 Mac 中使用 Mail 发邮件的时候，可以在签名框的后面看到星标和锁型标识。

星标为是否可以签名，锁型标识表示是否可以加密传输。

让有证书的朋友给你发送签名邮件，即可得到朋友的公钥，导入到你的系统后，就可以给他发送加密邮件了。

点击锁型图标为锁上的发送的邮件就是加密的，只有你的朋友才能解密看到内容。

以后发送重要信息就可以使用这种途径发送，才能保证秘密呀。

### 3. 在其他设备上面使用

如果在其他设备上面使用，可以从 Keychain Access （钥匙串访问）里面导出为.p12格式的文件。

然后发送到其他设备，导入即可。

### 4. 其他客户端

- airmail 需要安装1.5版本[beta] S/MIME 插件 http://airmailapp.com/smime/ 目前有bug
- 雷鸟没有用过，不太清楚
