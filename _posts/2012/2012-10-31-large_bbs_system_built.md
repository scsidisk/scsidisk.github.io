---
layout: post
title: "关于大型论坛系统环境搭建"
date: 2012-10-31
author: scsidisk
category: PHP
tags: [Nginx, PHP, MySQL, Apache]
---

关于大型论坛系统环境搭建(20万日IP负载平衡实战)-Nginx+Apache2+PHP+MySQL

（本文只针对Discuz论坛系统讨论，由于软件包更新速度比较快，你看本贴的时候可能已经是使用新版本的软件包了，安装方法可能不一致，详细请查看软件包的README文件）

### 测试环境:理想论坛(55188).

理想论坛为国内人气最旺的股票论坛，注册会员已超过100万，并以每月60000人的速度稳定递增，每日页面访问量超过200万，并保持稳定增长的趋势,60分钟在线平均约2万多人，最高记录3万3千多。 目前主题超过30万，帖子接近1千万，数据库大小5.8GB，附件总大小大约150GB

之前理想论坛有三台服务器，两台WEB服务器以及一台数据库服务器，访问已经渐渐出现瓶颈，在猪头的建议下，站长决定增加一台服务器放数据库，另外三台做WEB，并且对原有的服务器的操作系统进行升级。

### 硬件具体情况

MySQL服务器： DualXeon 5335/8GB内存/73G SAS硬盘（RAID0+1）/CentOS5.1-x86_64/MySQL5

三台WEB服务器如下：

N1. Dual Xeon 3.0 2GB 内存
N1. Dual Xeon 3.0 4GB 内存
N1. Dual Xeon 3.0(双核) 4G内存

另外有三块300G的SCSI硬盘准备做RAID5，用来存放附件,四台机器通过内网连接

猪头考虑过的解决方案如下:

1. ZEUS + PHP5 + eAccelerator
2. squid + Apache2 + PHP + eAccelerator
3. nginx + PHP(fastcgi) + eAccelerator
4. nginx + Apache2 + PHP + eAccelerator

第一个方案，属于比较完美的，而且很稳定，但是最大的问题是ZEUS是收费软件，用盗版总会受良心责备的,所以暂时押后做候补方案

第二个方案，squid转发请求给Apache2，很多网站都采用这种方式，而且效率也非常高，猪头也测试了一下，但是问题非常严重，因为squid是把文件缓存起来的，所以每一个访问过的文件，squid都要把它打开，理想论坛拥有150G的附件，而且访问量巨大，这种情况下只有打开squid，机器很快就会因为打开文件过多而拒绝响应任何请求了,看来也不适合,只适合缓存文件只有几百M以内的网站.

第三个方案，猪头对第三个方案的测试结果是访问量大的时候，PHP经常会出现bad gateway，看来通过TCP连接Fastcgi执行PHP的方法不够稳定，猪头也测试了通过Unix Socket连接执行PHP，同样还是不稳定.

对比之下，猪头目前使用了第四种解决方案.

### Apache2的安装。

(由于服务器采用FreeBSD7，所以大部分软件将会通过ports安装)

由于Apache2只需要处理PHP请求，所以其他模块基本上都不需要，所以不要选择安装其他模块，即使rewrite也不需要，因为rewrite将会在nginx上面实现，如果熟悉，还可以修改Makefile删掉不需要的部分，这样经过优化之后，apache将会以最稳定最高效的方式处理PHP请求

```
cd /usr/ports/www/apache20
make install clean
```

修改httpd.conf(这里仅列出要修改/增加的部分)

```
vi /usr/local/etc/apache2/httpd.conf
```

把KeepAlive On修改为KeepAlive Off，在下面添加

```apache
ServerLimit 2048
# MaxClients增加到512
Listen 127.0.0.1:81 #由于httpd服务器不需要对外开放，仅仅处理nginx转发过来的PHP请求，所以仅仅需要监听本地的端口.
```

另外增加对PHP的支持

```apache
AddType application/x-httpd-php .php
AddType application/x-httpd-php-source .phps
```

至于添加虚拟主机的部分将不再罗嗦，注意虚拟主机也监听本地81端口就可以了

### PHP5的安装(GD库等模块请提前装好)

```
cd /usr/ports/lang/php5
```

修改一下Makefile，把需要的东西加上去吧

本来应该有这样一段的

```make
CONFIGURE_ARGS= \
-with-layout=GNU \
-with-config-file-scan-dir=${PREFIX}/etc/php \
-disable-all \
-enable-libxml \
-with-libxml-dir=${LOCALBASE} \
-enable-reflection \
-program-prefix=""
```

我们要把它修改成

```make
CONFIGURE_ARGS= \
-with-layout=GNU \
-with-config-file-scan-dir=${PREFIX}/etc/php \
-disable-all \
-enable-libxml \
-with-libxml-dir=${LOCALBASE} \
-enable-reflection \
-program-prefix="" \
-with-config-file-path=/etc -enable-mbstring -enable-ftp -with-gd -with-jpeg-dir=/usr/local -with-png-dir=/usr/local -enable-magic-quotes -with-mysql=/usr/local -with-pear -enable-sockets -with-ttf -with-freetype-dir=/usr/local -enable-gd-native-ttf -with-zlib -enable-sysvsem -enable-sysvshm -with-libxml-dir=/usr/local -with-pcre-regex -enable-xml
```

安装

```
make install clean
cp work/php-5.2.5/php.ini-dist /etc/php.ini
```

### 安装eAccelerator

```
cd /usr/ports/www/eaccelerator
make install clean
```

把以下部分添加到php.ini尾端:

```ini
extension_dir="/usr/local/lib/php/20060613/"
extension="eaccelerator.so"
eaccelerator.cache_dir="/tmp/eaccelerator"
eaccelerator.shm_size="64"
eaccelerator.enable="1"
eaccelerator.optimizer="1"
eaccelerator.check_mtime="1"
eaccelerator.debug="0"
eaccelerator.filter=""
eaccelerator.shm_max="0"
eaccelerator.shm_ttl="60"
eaccelerator.shm_prune_period="60"
eaccelerator.shm_only="0"
eaccelerator.compress="1"
eaccelerator.compress_level="9"
eaccelerator.keys="shm_and_disk"
eaccelerator.sessions="shm_and_disk"
eaccelerator.content="shm_and_disk"
```

建立缓存目录以及修改权限

```
mkdir /tmp/eaccelerator
chmod 777 /tmp/eaccelerator
chown nobody:nobody /tmp/eaccelerator
```

### nginx的安装以及配置

```
cd /usr/ports/www/nginx
make install
```

有几个module是我们需要的，要选上

```
HTTP module
http_addition module
http_rewrite module
http_realip module
http_stub_status module
```

其他的看自己需要了

修改配置文件

```
vi /usr/local/etc/nginx/nginx.conf
```

```nginx
user nobody nobody;
worker_processes 4;
#error_log logs/error.log;
#error_log logs/error.log notice;
#error_log logs/error.log info;
#pid /var/log/nginx.pid;
events {
	worker_connections 10240;
}
http {
	include mime.types;
	default_type application/octet-stream;
	limit_zone one $binary_remote_addr 10m;
	#log_format main '$remote_addr - $remote_user [$time_local] $request '
	# '"$status" $body_bytes_sent “$http_referer" '
	# '"$http_user_agent" “$http_x_forwarded_for"';
	sendfile off;
	tcp_nopush off;
	#keepalive_timeout 0;
	keepalive_timeout 10;
	gzip off;
	server {
		listen 80;
		server_name www.55188.net www.55188.com www1.55188.com www2.55188.com 55188.com 55188.net www.55188.cn 55188.cn bbs.55188.net bbs.55188.com bbs.55188.cn;
		index index.html index.htm index.php;
		root /home/www;
		access_log /dev/null combined;
		limit_conn one 5;#限制一个IP并发连接数为五个
		error_page 404 /404.html;
		error_page 403 /403.html;
		location /status {
			stub_status on;
			access_log off;
			auth_basic “NginxStatus";
			auth_basic_user_file conf/htpasswd;
		}
		#在根目录使用Discuz6.0 rewrite规则,如果你的论坛在二级目录下面,则要相应修改location
		location / {
			rewrite ^/archiver/((fid|tid)-[\w\-]+\.html)$ /archiver/index.php?$1 last;
			rewrite ^/forum-([0-9]+)-([0-9]+)\.html$ /forumdisplay.php?fid=$1&page=$2 last;
			rewrite ^/thread-([0-9]+)-([0-9]+)-([0-9]+)\.html$ /viewthread.php?tid=$1&extra=page\%3D$3&page=$2 last;
			rewrite ^/space-(username|uid)-(.+)\.html$ /space.php?$1=$2 last;
			rewrite ^/tag-(.+)\.html$ /tag.php?name=$1 last;
			break;
			error_page 404 /404.html;
			error_page 403 /403.html;
		}
		#对附件做防盗链,没有正确的referer将会返回403页面
		location ~* ^.+\.(gif|jpg|png|swf|flv|rar|zip|doc|pdf|gz|bz2|jpeg|bmp|xls)$ {
			valid_referers none blocked server_names *.55188.net *.55188.com;
			if ($invalid_referer) {
				rewrite ^/ http://www.55188.com/403.html;
			}
		}
		#转发PHP请求到本地的81端口,让Apache处理.
		location ~ \.php$ {
			proxy_pass http://127.0.0.1:81;
			proxy_redirect off;
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_hide_header Content-Type;
		}
	}
}
```

测试一下你的配置文件是否都正确

```
/usr/local/sbin/apachectl configtest
/usr/local/sbin/nginx -t
```

都没问题的话就启动服务器吧

```
/usr/local/sbin/apachectl start
/usr/local/sbin/nginx -c /usr/local/etc/nginx/nginx.conf
```

浏览一下主页，应该正常了

### 后继讨论，

#### 1.数据库.

数据库的编译安装不再重复讨论，仅仅讨论环境，由于理想论坛的数据库比较大，而且发展比较快，所以要作比较前一点的预算，硬盘需要使用15K RPM的SAS硬盘做RAID0+1，操作系统需要使用64位版本，因为服务器需要8GB内存，要注意的时，使用了64位系统之后部分比较老的软件可能你无法找到64位的版本，这台机器就专门做MySQL服务器吧，如果数据库超过10G，应该考虑MySQL_Cluster

#### 2.附件.

因为有三台服务器做WEB，所以附件要使用nfs的方式通过内网进行共享，至于如何设置nfs这里不再讨论，如果有不明白的请将学费交给Google

#### 3.WEB.

由于三台机器硬件配置不一致,所以有必要考虑一下负载平衡的问题,nginx本身附带有负载平衡的功能,但是如果启用负载平衡的功能的话,每台机器都将会把客户端请求的数据缓存到本机,这样增加了硬盘的IO,对于理想论坛的访问量来说,这是个不小的开销,最后我们是使用DNS查询的方式来分配流量, 通过不同的A记录,配置好点的机器，多分一条A记录，配置差的就少一条A记录，这样从整体上看，流量分配应该比较平衡.

#### 4.关于nginx并发连接

猪头给nginx限制了每个IP的并发连接，因为对于大论坛来说，总是比较出名的，不说人家攻击你什么的.采集都特别多,如果不限制,很容易出问题,经常会导致PHP罢工.
以上只是猪头愚见，如果有其他进展，猪头会更新本贴，如有疑问或者不同见解，欢迎提出讨论
当然还有很多很疯狂的方法，例如说把WEB文件(附件除外)全部放内存里面，MYSQL如果小于5G，也可以全部放内存里面，不过这些方法都是太极端的了，优化效果须然好，但是风险很大。

### 优化之后的效果

由于还有两台机器升级没完成，只帖一下其中一台WEB的状况了。目前

```
Active connections: 1143
server accepts handled requests
1211445 1211445 6221785
Reading: 67 Writing: 136 Waiting: 940
```

Apache最优化要关闭不用的模块，因为httpd请求全部让nginx处理了，Apache仅仅需要处理PHP就可以了，目前我开启的模块

```apache
LoadModule access_module libexec/apache2/mod_access.so
LoadModule setenvif_module libexec/apache2/mod_setenvif.so
LoadModule mime_module libexec/apache2/mod_mime.so
LoadModule autoindex_module libexec/apache2/mod_autoindex.so
LoadModule negotiation_module libexec/apache2/mod_negotiation.so
LoadModule alias_module libexec/apache2/mod_alias.so
LoadModule rewrite_module libexec/apache2/mod_rewrite.so
LoadModule php5_module libexec/apache2/libphp5.so
```

autoindex negotiation以及rewrite这些应该都关闭的，但是要做相应的修改.目前跑起来绝对比Fastcgi要好
