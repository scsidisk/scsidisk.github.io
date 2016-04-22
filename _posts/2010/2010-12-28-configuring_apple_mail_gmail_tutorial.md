---
layout: post
title: "Apple Mail配置GMail教程"
date: 2010-12-28 14:54
author: scsidisk
category: MacOSX
tags: [Mac]
Slug: configuring_apple_mail_gmail_tutorial
---

在MAC OS
X系统上配置GMail客户端，其实还是需要一定技巧的。对于用GMail不多的网友来说，就用Mail程序作为POP方式客户端简单收取邮件是足够了，但是如果用作主用邮箱，设置了标签，订阅了邮件列表等等，就会发现使用默认功能总是很不顺手，似乎不如直接使用GMail

WEB方式强大。真的是因为苹果自带的邮件程序很鸡肋么？不，让我们通过简单的配置解决问题吧。

首先，要在GMail的WEB页面上点击“Setting——Forwarding and POP/IMAP”，按照下图设置，主要是要改用IMAP方式：

[![68_91849_41ee9aebddf8bed](/images/2010/12/68_91849_41ee9aebddf8bed-300x188.jpg)](/images/2010/12/68_91849_41ee9aebddf8bed.jpg)

然后打开Mail，新增邮箱，此时只需要输入你的邮箱名和密码，会自动使用IMAP方式：

[![68_91849_a5cafb24d787e0d](/images/2010/12/68_91849_a5cafb24d787e0d-300x219.jpg)](/images/2010/12/68_91849_a5cafb24d787e0d.jpg)

第三步，进入Mail的偏好设置（Command＋逗号），在帐户处选择你刚刚建立的GMail，按照下图[设置邮箱行为](http://mail.google.com/support/bin/answer.py?answer=78892#)：

[![68_91849_9e586ed1268116a](/images/2010/12/68_91849_9e586ed1268116a-248x300.jpg)](/images/2010/12/68_91849_9e586ed1268116a.jpg)

在建立好邮箱后你会在左下方看到GMail的不少标签或者分类什么的，还需要设置一下，才能让Mail里的这些分类或者说“子文件夹”与GMail真正同步起来，比如选择Drafts，然后点击Mail的“邮箱——这个邮箱用于——草稿”，同样将Spam关联到“垃圾”，Trash关联到“废纸篓”，Sent

Mail关联到“已发出邮件”。

[![4826115609_04cab77964](/images/2010/12/4826115609_04cab77964-159x300.jpg)](/images/2010/12/4826115609_04cab77964.jpg)

以上完成后，其实邮箱关系如下：

[![68_91849_96b88a2db5b1c7e](/images/2010/12/68_91849_96b88a2db5b1c7e-115x300.jpg)](/images/2010/12/68_91849_96b88a2db5b1c7e.jpg)

那么如何在客户端程序里归档邮件或者给邮件打标签呢？右键点击你要处理的邮件，移动到你需要的地方即可。其中移动到All

Mail就相当于WEB页面上的Archive动作；移动到某一个标签，比如Oracle-L（我订阅了很久的一个邮件列表，关于Oracle技术），那么就是给邮件打标签为Oracle-L，并归档。

[![4826754370_c91327b743](/images/2010/12/4826754370_c91327b743-300x277.jpg)](/images/2010/12/4826754370_c91327b743.jpg)

使用IMAP同步后，仍然需要注意，有些操作的关联和一般的使用有所不同，下表翻译自[GMail官方说明](http://mail.google.com/support/bin/answer.py?answer=77657)，供参考。总体来说，就是客户端的目录对应于网页的标签（Label）。

<table border="1">
<tbody>
<tr>
<th>
移动设备和客户端侧动作(如iPhone/Outlook)

</th>
<th>
Gmail Web页面结果

</th>
</tr>
<tr>
<td>
打开信息

</td>
<td>
标记信息为已读

</td>
</tr>
<tr>
<td>
标记信息

</td>
<td>
信息加星标

</td>
</tr>
<tr>
<td>
移动信息至一个文件夹

</td>
<td>
信息打上对应标签

</td>
</tr>
<tr>
<td>
移动信息至一个子文件夹*

</td>
<td>
信息打上层次标签 (‘主目录/子目录’)*

</td>
</tr>
<tr>
<td>
创建文件夹

</td>
<td>
新建标签

</td>
</tr>
<tr>
<td>
移动信息至 [Gmail]/Spam

</td>
<td>
信息移入Spam

</td>
</tr>
<tr>
<td>
移动信息至 [Gmail]/Trash

</td>
<td>
移动信息至Trash

</td>
</tr>
<tr>
<td>
发送信息

</td>
<td>
在发件夹中存储信息

</td>
</tr>
<tr>
<td>
在收件箱中删除信息**

</td>
<td>
从收件箱删除信息（邮件本身还是存在）**

</td>
</tr>
<tr>
<td>
从文件夹删除信息**

</td>
<td>
从信息删除标签**

</td>
</tr>
<tr>
<td>
从[Gmail]/Spam或[Gmail]/Trash删除信息

</td>
<td>
永久删除信息

</td>
</tr>
</tbody>
</table>
<div class="posttagsblock">
</div>

