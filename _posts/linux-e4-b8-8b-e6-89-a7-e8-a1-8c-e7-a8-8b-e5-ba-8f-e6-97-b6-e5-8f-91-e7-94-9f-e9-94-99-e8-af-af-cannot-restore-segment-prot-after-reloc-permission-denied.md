Title: Linux下执行程序时发生错误: cannot restore segment prot after reloc: Permission denied
Date: 2010-12-28 14:57
Author: scsidisk
Category: CentOS
Tags: CentOS, Linux, Shell
Slug: linux%e4%b8%8b%e6%89%a7%e8%a1%8c%e7%a8%8b%e5%ba%8f%e6%97%b6%e5%8f%91%e7%94%9f%e9%94%99%e8%af%af-cannot-restore-segment-prot-after-reloc-permission-denied

<span style="font-family: 'STHeiti Light';">　　</span>
<span style="font-family: 'STHeiti Light';">原来这是<span style="font: 12.0px Monaco;">SELinux</span>搞的鬼，解决办法有两个</span>

<span style="font: 12.0px STHeiti Light;">　　</span> 1.
<span style="font: 12.0px STHeiti Light;">使用</span>chcon
<span style="font: 12.0px STHeiti Light;">命令</span>

<span style="font: 12.0px STHeiti Light;">　　</span>
<span style="font: 12.0px STHeiti Light;">示例</span>: chcon -t
texrel\_shlib\_t /usr/local/rsi/idl\_6.1/bin/bin.linux.x86/\*.so

<span style="font: 12.0px STHeiti Light;">　　</span> 2.
<span style="font: 12.0px STHeiti Light;">禁止掉</span>SELinux

<span style="font: 12.0px STHeiti Light;">　　</span>
<span style="font: 12.0px STHeiti Light;">更改</span>/etc/sysconfig/selinux
<span style="font: 12.0px STHeiti Light;">文件的内容为</span>
SELINUX=disabled

<div class="posttagsblock">
[ ](http://technorati.com/tag/CentOS)

</div>

