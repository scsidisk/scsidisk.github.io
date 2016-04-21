免费SSL安全证书Let's Encrypt安装使用教程(附Nginx/Apache配置)
--------------------------------------------------------

摘自: http://www.vpser.net/build/letsencrypt-free-ssl.html

2015年12月9日 上午 | 作者：VPS侦探


[![letsencrypt](http://vpsercdn.b0.upaiyun.com/uploads/2015/12/letsencrypt.png)](http://www.vpser.net/build/letsencrypt-free-ssl.html)

Let's Encrypt是最近很火的一个免费SSL证书发行项目，Let's
Encrypt是由ISRG提供的免费免费公益项目，自动化发行证书，但是证书只有90天的有效期。适合个人使用或者临时使用，不用再忍受自签发证书不受浏览器信赖的提示。前段时间一直是内测，现在已经开放了。本教程安装不需要停掉当前Web服务(Nginx/Apache)，直接生成证书，废话不多说下面开始：

建议使用git 以后有了新版更新方便，没安装的话Debian/Ubuntu：**apt-get
install git** ，CentOS：**yum install git-core**\
 **git clone https://github.com/letsencrypt/letsencrypt.git**\
 **cd letsencrypt**

不安装git的话：**wget
-c https://github.com/letsencrypt/letsencrypt/archive/master.zip &&
unzip master.zip && cd letsencrypt-master**

[LNMP一键安装包](http://lnmp.org)都是Nginx/Apache默认支持ssl不需要另外单独编译，接下来先以[LNMP一键安装包](http://lnmp.org)为例，LNMP用户可以直接参考此教程：

执行：**mkdir -p /home/wwwroot/域名/.well-known/acme-challenge**
创建临时目录，当然这个.well-known/acme-challenge前面的目录要替换为你自己的网站目录，根据你自己的实际情况修改。

### 正式开始生成证书

接下来正式进行证书生成操作：\
 **./letsencrypt-auto certonly --email 邮箱 -d 域名 --webroot -w
/网站目录完整路径 --agree-tos**\
 如果多个域名可以加多个-d
域名，注意替换上面的邮箱、域名和网站目录，注意这里的网站目录完整路径只是你单纯的网站目录也就是虚拟主机配置文件里的，如Nginx虚拟主机配置里的root，Apache虚拟主机配置里的DocumentRoot。

首先Let's
Encrypt会检测系统安装一些依赖包，安装完依赖包会有~~蓝色的让阅读TOS的提示，Agree回车
稍等片刻就行了~~可添加--agree-tos参数屏蔽该窗口。\
 生成证书后会有如下提示：

> IMPORTANT NOTES:\
>  - If you lose your account credentials, you can recover through\
>  e-mails sent to licess@vpser.net.\
>  - Congratulations! Your certificate and chain have been saved at\
>  /etc/letsencrypt/live/www.vpser.net/fullchain.pem. Your cert will\
>  expire on 2016-03-07. To obtain a new version of the certificate in\
>  the future, simply run Let's Encrypt again.\
>  - Your account credentials have been saved in your Let's Encrypt\
>  configuration directory at /etc/letsencrypt. You should make a\
>  secure backup of this folder now. This configuration directory will\
>  also contain certificates and private keys obtained by Let's\
>  Encrypt so making regular backups of this folder is ideal.\
>  - If like Let's Encrypt, please consider supporting our work by:
>
> Donating to ISRG / Let's Encrypt: https://letsencrypt.org/donate\
>  Donating to EFF: https://eff.org/donate-le

### Nginx虚拟主机的设置

接下来进行配置Nginx虚拟主机文件，完整配置如下：

> server\
>  {\
>  listen 443 ssl;  
> //如果需要spdy也可以加上,lnmp1.2及其后版本都默认支持spdy,lnmp1.3 nginx
> 1.9.5以上版本默认支持http2\
>  server\_name www.vpser.net;     //这里是你的域名\
>  index index.html index.htm index.php default.html default.htm
> default.php;\
>  root /home/wwwroot/www.vpser.net;            //网站目录\
>  ssl\_certificate /etc/letsencrypt/live/www.vpser.net/fullchain.pem;  
>  //前面生成的证书，改一下里面的域名就行，不建议更换路径\
>  ssl\_certificate\_key
> /etc/letsencrypt/live/www.vpser.net/privkey.pem;  
> //前面生成的密钥，改一下里面的域名就行，不建议更换路径\
>  ssl\_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";\
>  ssl\_protocols TLSv1 TLSv1.1 TLSv1.2;\
>  ssl\_prefer\_server\_ciphers on;\
>  ssl\_session\_cache shared:SSL:10m;
>
> include wordpress.conf;  //这个是伪静态根据自己的需求改成其他或删除\
>  \#error\_page 404 /404.html;\
>  location \~ \[\^/\]\\.php(/|\$)\
>  {\
>  \# comment try\_files \$uri =404; to enable pathinfo\
>  try\_files \$uri =404;\
>  fastcgi\_pass unix:/tmp/php-cgi.sock;\
>  fastcgi\_index index.php;\
>  include fastcgi.conf;     //lnmp 1.0及之前版本替换为include
> fcgi.conf;\
>  \#include pathinfo.conf;\
>  }
>
> location \~ .\*\\.(gif|jpg|jpeg|png|bmp|swf)\$\
>  {\
>  expires 30d;\
>  }
>
> location \~ .\*\\.(js|css)?\$\
>  {\
>  expires 12h;\
>  }
>
> access\_log off;\
>  }

需将上述配置根据自己的实际情况修改后，添加到虚拟主机配置文件最后面。

添加完需要执行：**/etc/init.d/nginx reload** 重新载入配置使其生效。

如果需要HSTS，可以加上add\_header Strict-Transport-Security
"max-age=63072000; includeSubdomains; preload";

**Apache虚拟主机上的设置**

Apache在生成证书后也需要修改一下apache的配置文件
/usr/local/apache/conf/httpd.conf ，查找httpd-ssl将前面的\#去掉。

然后再执行：\
 cat &gt;/usr/local/apache/conf/extra/httpd-ssl.conf&lt;&lt;EOF\
 Listen 443

AddType application/x-x509-ca-cert .crt\
 AddType application/x-pkcs7-crl .crl

SSLCipherSuite EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH\
 SSLProxyCipherSuite EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH\
 SSLHonorCipherOrder on

SSLProtocol all -SSLv2 -SSLv3\
 SSLProxyProtocol all -SSLv2 -SSLv3\
 SSLPassPhraseDialog builtin

SSLSessionCache "shmcb:/usr/local/apache/logs/ssl\_scache(512000)"\
 SSLSessionCacheTimeout 300

SSLMutex "file:/usr/local/apache/logs/ssl\_mutex"\
 EOF

并在对应apache虚拟主机配置文件的<span
style="line-height: 1.5;">最后&lt;/VirtualHost&gt;下面添加上SSL部分的配置文件：</span>

> &lt;VirtualHost \*:443&gt;\
>  DocumentRoot /home/wwwroot/www.vpser.net   //网站目录\
>  ServerName www.vpser.net:443   //域名\
>  ServerAdmin licess@vpser.net      //邮箱\
>  ErrorLog "/home/wwwlogs/www.vpser.net-error\_log"   //错误日志\
>  CustomLog "/home/wwwlogs/www.vpser.net-access\_log" common  
>  //访问日志\
>  SSLEngine on\
>  SSLCertificateFile /etc/letsencrypt/live/www.vpser.net/fullchain.pem
>   //改一下里面的域名就行，不建议更换路径\
>  SSLCertificateKeyFile /etc/letsencrypt/live/www.vpser.net/privkey.pem
>    //改一下里面的域名就行，不建议更换路径\
>  &lt;Directory "/home/wwwroot/www.vpser.net"&gt;   //网站目录\
>  SetOutputFilter DEFLATE\
>  Options FollowSymLinks\
>  AllowOverride All\
>  Order allow,deny\
>  Allow from all\
>  DirectoryIndex index.html index.php\
>  &lt;/Directory&gt;\
>  &lt;/VirtualHost&gt;

需将上述配置根据自己的实际情况修改后，添加到虚拟主机配置文件最后面。注意要重启apache使其实现。执行：**/etc/init.d/httpd
restart** 重启Apache使其生效。

### 下面说一下可能会遇到的问题：

#### 1、国内DNS服务商可能会不行，目前已知dnspod、cloudxns不行

Namecheap、Route 53的都可以。

#### 2、Linode福利或IPv6用户福利

可能目前Linode用户应该遇到了\
 An unexpected error occurred:\
 There were too many requests of a given type :: Error creating new
registration :: Too many registrations from this IP\
 Please see the logfiles in /var/log/letsencrypt for more details.\

这个不一定是因为IP注册的次数过多，可能是因为IPv6的事，具体解决方法如下：\
 执行：**sysctl -w net.ipv6.conf.all.disable\_ipv6=1** 来临时禁用IPv6\
 再生成证书后执行：**sysctl -w net.ipv6.conf.all.disable\_ipv6=0**
再来解除禁用IPv6

### 证书续期

最后要说的是续期，因为证书只有90天，所以建议60左右的时候进行一次续期，续期很简单可以交给[crontab](http://www.vpser.net/manage/crontab.html)进行完成，执行：

**cat &gt;/root/renew-ssl.sh&lt;&lt;EOF**\
 **\#!/bin/bash**\
 **mkdir -p /网站目录完整路径/.well-known/acme-challenge**\
 **/root/letsencrypt/letsencrypt-auto --renew-by-default certonly
--email 邮箱 -d 域名 --webroot -w /网站目录完整路径 --agree-tos**\
 **EOF**\
 **chmod +x /root/renew-ssl.sh**

注意要修改上面letsencrypt-auto的路径为你自己的，并且里面的邮箱和域名也要修改。\
 再crontab里添加上：**0 3 \*/60 \* \*
/root/renew-ssl.sh** 具体[crontab教程点击查看](http://www.vpser.net/manage/crontab.html)

2015.12.13更新\
 官网更新了参数，对本文进行了部分参数的调整。原-a webroot
--webroot-path=/网站目录完整路径替换为--webroot
-w；--renew替换为--renew-by-default；增加--agree-tos参数。

**同时这里提醒一下如果设置了http
301跳到https的用户，再续期前还需要在nginx设置如下：**\
 80端口的虚拟主机上需要添加上，不加的话会无法验证的

> location /.well-known/ {\
>  add\_header Content-Type 'text/plain;';\
>  root /网站目录完整路径;\
>  }

附完整的nginx下301 http跳到https的配置：

> server\
>  {\
>  listen 80;\
>  server\_name www.vpser.net;\
>  location /.well-known/ {\
>  add\_header Content-Type 'text/plain;';\
>  root /网站目录完整路径;\
>  }\
>  location / {\
>  return 301 https://www.vpser.net\$request\_uri;\
>  }\
>  }

目前就先说这些，有问题可以在本文章下部留言或到[VPS论坛](http://bbs.vpser.net)发帖。

