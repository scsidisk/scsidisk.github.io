Title: Linux修改SSH端口和禁止Root远程登陆设置
Date: 2010-12-28 14:56
Author: scsidisk
Category: CentOS
Tags: CentOS, Linux, SSH
Slug: linux%e4%bf%ae%e6%94%b9ssh%e7%ab%af%e5%8f%a3%e5%92%8c%e7%a6%81%e6%ad%a2root%e8%bf%9c%e7%a8%8b%e7%99%bb%e9%99%86%e8%ae%be%e7%bd%ae

Linux修改ssh端口22  
vi /etc/ssh/ssh\_config  
vi /etc/ssh/sshd\_config  
然后修改为port 8888

以root身份service sshd restart (redhat as3)

Linux下SSH默认的端口是22,为了安全考虑，现修改SSH的端口为1433,修改方法如下
：

/usr/sbin/sshd -p 1433

为增强安全  
先增加一个普通权限的用户：  
\#useradd uploader  
\#passwd uploader  
//设置密码  
生产机器禁止ROOT远程SSH登录：

\#vi /etc/ssh/sshd\_config  
把  
PermitRootLogin yes  
改为  
PermitRootLogin no

重启sshd服务  
\#service sshd restart

远程管理用普通用户uploader登录，然后用 su root
切换到root用户拿到最高权限。
