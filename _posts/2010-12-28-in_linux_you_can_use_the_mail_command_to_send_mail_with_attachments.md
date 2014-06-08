Title: 在Linux中用mail命令发送带附件的邮件
Date: 2010-12-28 13:51
Author: scsidisk
Category: CentOS
Tags: CentOS, Linux
Slug: in_linux_you_can_use_the_mail_command_to_send_mail_with_attachments

> 1、用uuencode 将附件编码为文本形式
>
> uuencode 附件 希望在邮件中使用的附件名 \> 附件文本文件
>
> 2、连接邮件正文文件和附件文本文件
>
> cat 邮件正文文件 附件文本文件 \> 正文附件联合文件
>
> 3、发送该邮件
>
> mail -s "你想使用的邮件标题" 收信人email地址 \< 正文附件联合文件
>
> 示例：
>
> 我的邮件正文文件是 message.txt，想发送的附件名字是
> attachment.tar.gz，附件文本文件取名为attachment.txt，正文附件联合文件取名为combined.txt，以“测试”为邮件标题发信给Sam@test.com
>
> uuencode attachment.tar.gz attachment.tar.gz \> attachment.txt  
>  cat message.txt attachment.txt \> combined.txt  
>  mail -s "测试‘ Sam@test.com \< combined.txt
>
> [From
> [<cite>在Linux中用mail命令发送带附件的邮件－站长资讯网info110.com</cite>](http://www.info110.com/mailserver/in23366-1.htm)]

<div class="posttagsblock">
[CentOS](http://technorati.com/tag/CentOS),
[Linux](http://technorati.com/tag/Linux),
[mail](http://technorati.com/tag/mail),
[邮件](http://technorati.com/tag/%E9%82%AE%E4%BB%B6)

</div>

