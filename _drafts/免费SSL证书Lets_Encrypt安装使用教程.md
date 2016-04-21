免费SSL证书Let’s Encrypt安装使用教程
==================================

http://www.tennfy.com/3911.html


去年就听说过Let’s Encrypt要出免费的SSL证书，可谓期待已久。12月3号Let’s
Encrypt正式开始公测，相信很多朋友也忍不住要来尝试一下。本文就来介绍下Let’s
Encrypt在debian系统上的安装使用教程。

Let’s Encrypt介绍
-----------------

Let’s Encrypt是国外一个公共的免费SSL项目，由 Linux
基金会托管，由Mozilla、思科、Akamai、IdenTrust和EFF等组织发起，目的就是向网站自动签发和管理免费证书，以便加速互联网由HTTP过渡到HTTPS，目前Facebook等大公司开始加入赞助行列。

Let’s Encrypt已经得了 IdenTrust
的交叉签名，这意味着其证书现在已经可以被Mozilla、Google、Microsoft和Apple等主流的浏览器所信任，你只需要在Web
服务器证书链中配置交叉签名，浏览器客户端会自动处理好其它的一切，Let’s
Encrypt安装简单，未来大规模采用可能性非常大。

由于处于公测期间，签发的证书只有90天的有效期，到期之后需要重新签发。

官方网站：<https://letsencrypt.org/>
 项目主页：<https://github.com/letsencrypt/letsencrypt>

Let’s Encrypt安装方法
---------------------

### 1、安装git并获取安装脚本

安装git：

    apt-get install git

下载安装脚本

    git clone https://github.com/letsencrypt/letsencrypt
    cd letsencrypt

### 2、获取Let’s Encrypt SSL证书

实际上，Let’s Encrypt 是通过访问域名实现对域名所有权的验证，因此需要vps上开放80端口。所以需要区分vps是否已安装nginx或apache等web服务器。

1）未安装nginx或apache等web服务器

如果vps上没有安装nginx或apache等web服务器，那么Let’s Encrypt脚本会自动开一个SimpleHTTPServer，通过该server运行Let’s Encrypt服务器来验证域名所有权。

执行以下命令获取SSL证书

    ./letsencrypt-auto certonly --standalone --email admin@thing.com -d thing.com

其中admin@thing.com为你的邮箱，thing.com为待签发的域名。

2）已安装nginx或apache等web服务器

如果vps上已安装nginx或apache等web服务器，则需要指定该域名在web服务器中对应的虚拟主机目录。

执行以下命令获取SSL证书

    ./letsencrypt-auto certonly --standalone --email admin@thing.com -d thing.com --webroot-path=/var/www/thing.com

其中admin@thing.com为你的邮箱，thing.com为待签发的域名，/var/www/thing.com为web服务器中定义的虚拟主机目录。

执行上述命令后，会弹出对话框，同意用户协议。

[![letsencrypt1](http://tennfy.qiniudn.com/wp-content/uploads/2015/12/letsencrypt1-600x361.png)](http://tennfy.qiniudn.com/wp-content/uploads/2015/12/letsencrypt1.png)

如果运行没有错误，那么相应的证书会生成在/etc/letsencrypt/live/域名文件夹下，路径分别为：

    #certificate
    /etc/letsencrypt/live/域名/fullchain.pem
    #privatekey
    /etc/letsencrypt/live/域名/privkey.pem


### 3、遇到问题及解决方法

1）Error: The server could not connect to the client for DV

如果你的域名使用的是国内的NS服务商，比如：hichina、dnspod、cloudxns，可能不能正常通过验证

解决方案是将域名的NS服务商换为国外的，例如namecheap、
[linode](http://www.tennfy.com/tag/linode "linode")、cloudflare、google
cloud DNS。

2）IPV6用户问题

 可能目前 [linode](http://www.tennfy.com/tag/linode "linode")用户应该遇到了

```
An unexpected error occurred:
There were too many requests of a given type :: Error creating new registration ::
Too many registrations from this IP
Please see the logfiles in /var/log/letsencrypt for more details.
```

这个不一定是因为IP注册的次数过多，可能是因为IPv6的事，具体解决方法如下：

执行：sysctl -w net.ipv6.conf.all.disable\_ipv6=1 来临时禁用IPv6

再生成证书后执行：sysctl -w net.ipv6.conf.all.disable\_ipv6=0

再来解除禁用IPv6。

90天证书续期
------------

Let’s Encrypt [免费SSL证书](http://www.tennfy.com/tag/%e5%85%8d%e8%b4%b9ssl%e8%af%81%e4%b9%a6 "查看 免费SSL证书 中的全部文章")有效期是90天，也就是每三个月你就得续期一次。执行以下代码即可自动替换证书为新的：

    ./letsencrypt-auto --renew certonly --email admin@thing.com -d thing.com

其中admin@thing.com为你的邮箱，thing.com为待续期的域名。

本文永久链接: http://www.tennfy.com/3911.html


