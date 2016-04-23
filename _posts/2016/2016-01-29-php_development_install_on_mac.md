---
layout: post
title: "PHP开发环境 - PHP MySQL Nginx 安装"
date: 2016-01-29
author: scsidisk
categories: PHP
tags: PHP, Nginx, MySQL, MAMP, Memcache, Redis
---

在 Mac 上面进行 PHP 开发环境配置，不太麻烦。如果只是普通使用，可以安装 MAMP，也可以使用 brew 安装。当然也可以使用源码安装。

下面简单介绍 MAMP 和使用 brew 进行安装的过程。

安装 MAMP
---------

MAMP 套件包含 Apache，Nginx，PHP，MySQL，apc，phpmyadmin。

从 [官网](https://www.mamp.info/en/downloads/) 下载最新的安装包。进行安装

安装完成后，会在`应用程序`里面看到 MAMP ，双击执行。

执行后会在浏览器里面打开一个开始页面，默认web端口8080，mysql端口9999，可以设置成80和3306。

为了执行 php 命令行可以把 PHP 命令放到 `/usr/local/bin`. 确认 `/usr/local/bin` 在环境变量的前面。

    ln -sfv /Applications/MAMP/bin/php/php5.6.10/bin/php /usr/local/bin/php
    ln -sfv /Applications/MAMP/bin/php/php5.6.10/bin/phpize /usr/local/bin/phpize
    ln -sfv /Applications/MAMP/bin/php/php5.6.10/bin/php-config /usr/local/bin/php-config
    # 检查是否环境正确认
    ls -l `which php`

使用 brew 安装
-------------

### 检查 brew

如果 brew 没有安装可以使用下面命令安装。

    $ ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go/install)"

如果已经安装可以更新到最新

    $ brew -v
    $ brew doctor
    $ brew update

### 安装PHP5.6（FPM方式）

#### 添加源

    $ brew tap homebrew/dupes
    $ brew tap homebrew/versions
    $ brew tap homebrew/php

#### 安装 php

PHP 默认安装，会编译 Apache 环境下 mod_php 模块，为了使用 Nginx ，这里需要编译 php-fpm 并且禁用 apache ， 主要通过参数 `--without-fpm` `--without-apache` 来实现。 完整的安装指令为

    $ brew install php56 \
    --without-snmp \
    --without-apache \
    --with-debug \
    --with-fpm \
    --with-intl \
    --with-homebrew-curl \
    --with-homebrew-libxslt \
    --with-homebrew-openssl \
    --with-imap \
    --with-mysql \
    --with-tidy

如果你以前设置过 `/usr/local/bin/php` ，则安装后连接不成功。可以使用下面命令强制创建连接。

    $ brew link --overwrite php56

php.ini 位置在 `/usr/local/etc/php/5.6/php.ini`

请确认 `.bash_profile` 文件中有下面的设置。

    PATH="/usr/local/bin:/usr/local/sbin:$PATH"

测试一下效果：

    # brew安装的php 他在/usr/local/opt/php55/bin/php
    $ php -v
    # Mac自带的PHP
    $ /usr/bin/php -v
    # brew安装的php-fpm 他在/usr/local/opt/php55/sbin/php-fpm
    $ php-fpm -v
    # Mac自带的php-fpm
    $ /usr/sbin/php-fpm -v

#### 安装 PHP 扩展

    $ brew install igbinary
    # 为了和php保持版本上的一致，一定要使用源码编译安装扩展
    $ brew install --build-from-source php56-apcu \
        php56-mcrypt \
        php56-xdebug \
        php56-yaf \
        php56-memcached \
        php56-memcache \
        php56-mongo \
        php56-phalcon \
        php56-gmagick \
        php56-geoip \
        php56-redis

    ## 查看安装后的路径
    $ brew --prefix php56-xdebug
    ## 查看配置信息
    $ brew info php56-xdebug

配置 xdebug

    $ vim /usr/local/etc/php/5.6/conf.d/ext-xdebug.ini
    ## 添加下面内容
    [xdebug]
    zend_extension="/usr/local/opt/php56-xdebug/xdebug.so"
    xdebug.remote_enable = 1
    ; use port 9009 because php-fpm uses 9000 by default
    xdebug.remote_port = 9009

配置 timezone

    $ vim /usr/local/etc/php/5.6/conf.d/ext-xdebug.ini
    添加下面内容
    [Date]
    date.timezone = "Asia/Harbin"

修改 php-fpm 配置文件

    $ vim /usr/local/etc/php/5.5/php-fpm.conf
    # 去掉注释，pid会产生在/usr/local/var/run/php-fpm.pid, 下面的 Nginx pid 文件也放在这里
    pid = run/php-fpm.pid

#### 测试执行

    #测试php-fpm配置
    $ php-fpm -t
    #启动php-fpm
    $ php-fpm -D
    #关闭php-fpm
    $ killall php-fpm
    # 将php-fpm加入开机启动
    $ ln -sfv /usr/local/opt/php56/*.plist ~/Library/LaunchAgents
    # 启动
    $ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.php56.plist

启动php-fpm之后，确保它正常运行监听9000端口：

    $ lsof -Pni4 | grep LISTEN | grep php
    $ lsof -i tcp:9000

### 安装 php composer

    $ brew install composer
    $ composer --version

### 安装 Nginx

    $ brew install nginx
    # 如果使用 geoip 模块
    $ brew install nginx --with-http_geoip_module
    Docroot is: /usr/local/var/www
    default port has been set in /usr/local/etc/nginx/nginx.conf to 8080
    # 添加到自动启动
    $ ln -sfv /usr/local/opt/nginx/*.plist ~/Library/LaunchAgents
    # 启动
    $ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist

nginx安装后默认监听8080端口，可以访问 http://localhost:8080 查看状态。如果要想监听 8 0端口需要 root 权限，运行

    $ ls -l /usr/local/Cellar/nginx/1.8.0/bin/nginx
    -r-xr-xr-x  1 ccc  admin   689K Jan 29 11:04 /usr/local/Cellar/nginx/1.8.0/bin/nginx
    $ sudo chown root:wheel /usr/local/Cellar/nginx/1.8.0/bin/nginx
    $ sudo chmod u+s /usr/local/Cellar/nginx/1.8.0/bin/nginx
    $ vim /usr/local/etc/nginx/nginx.conf
    # 修改监听端口
    listen       80;

Nginx启动关闭命令：

    #测试配置是否有语法错误
    $ sudo nginx -t
    # 打开 nginx
    $ sudo nginx
    # 重新加载配置|重启|停止|退出 nginx
    $ sudo nginx -s reload|reopen|stop|quit

### Nginx + PHP-FPM配置

创建需要用到的目录：

    $ mkdir -p /usr/local/var/logs/nginx
    $ mkdir -p /usr/local/etc/nginx/sites-available
    $ mkdir -p /usr/local/etc/nginx/sites-enabled
    $ mkdir -p /usr/local/etc/nginx/conf.d
    $ mkdir -p /usr/local/etc/nginx/ssl
    $ sudo mkdir -p /var/www
    $ sudo chown :staff /var/www
    $ sudo chmod 775 /var/www

编辑 nginx 配置

    $ vim /usr/local/etc/nginx/nginx.conf

添加输入以下内容：

    worker_processes  1;

    error_log   /usr/local/var/logs/nginx/error.log debug;

    pid         /usr/local/var/run/nginx.pid;

    events {
        worker_connections  256;
    }

    http {
        include       mime.types;
        default_type  application/octet-stream;

        log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for"';


        access_log  /usr/local/var/logs/access.log  main;

        sendfile           on;
        keepalive_timeout  65;
        port_in_redirect   off;

        include /usr/local/etc/nginx/sites-enabled/*;
    }

这样一来首先可以把一些可复用配置独立出来放在 /usr/local/etc/nginx/conf.d 下，比如fastcgi的设置就可以独立出来

设置nginx php-fpm配置文件

    $ vim /usr/local/etc/nginx/conf.d/php-fpm
    #proxy the php scripts to php-fpm
    location ~ \.php$ {
        try_files                   $uri = 404;
        fastcgi_pass                127.0.0.1:9000;
        fastcgi_index               index.php;
        fastcgi_intercept_errors    on;
        include /usr/local/etc/nginx/fastcgi.conf;
    }

创建默认虚拟主机default

    $ vim /usr/local/etc/nginx/sites-available/default
    # 输入
    server {
        listen       80;
        server_name  localhost;
        root         /var/www/;

        access_log  /usr/local/var/logs/nginx/default.access.log  main;

        location / {
            index  index.html index.htm index.php;
            autoindex   on;
            include     /usr/local/etc/nginx/conf.d/php-fpm;
        }

        location = /info {
            allow   127.0.0.1;
            deny    all;
            rewrite (.*) /.info.php;
        }

        error_page  404     /404.html;
        error_page  403     /403.html;
    }

创建ssl默认虚拟主机default-ssl

    $ vim /usr/local/etc/nginx/sites-available/default-ssl
    # 输入：
    server {
        listen       443;
        server_name  localhost;
        root       /var/www/;

        access_log  /usr/local/var/logs/nginx/default-ssl.access.log  main;

        ssl                  on;
        ssl_certificate      ssl/localhost.crt;
        ssl_certificate_key  ssl/localhost.key;

        ssl_session_timeout  5m;

        ssl_protocols  SSLv2 SSLv3 TLSv1;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers   on;

        location / {
            include   /usr/local/etc/nginx/conf.d/php-fpm;
        }

        location = /info {
            allow   127.0.0.1;
            deny    all;
            rewrite (.*) /.info.php;
        }

        error_page  404     /404.html;
        error_page  403     /403.html;
    }

设置 SSL，生成自签名证书

    $ mkdir -p /usr/local/etc/nginx/ssl
    $ openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 -subj "/C=US/ST=State/L=Town/O=Office/CN=localhost" -keyout /usr/local/etc/nginx/ssl/localhost.key -out /usr/local/etc/nginx/ssl/localhost.crt

创建虚拟主机软连接，开启虚拟主机

    $ ln -sfv /usr/local/etc/nginx/sites-available/default /usr/local/etc/nginx/sites-enabled/default
    $ ln -sfv /usr/local/etc/nginx/sites-available/default-ssl /usr/local/etc/nginx/sites-enabled/default-ssl

接下来你可以通过下面这些连接访问：

    http://localhost/ -> index.html
    http://localhost/info.php -> info.php via phpinfo();
    http://localhost/404 -> 404.html
    https://localhost/ -> index.html(SSL)
    https://localhost/info.php -> info.php via phpinfo();(SSL)
    https://localhost/404 -> 404.html(SSL)

因为证书是自己生成的。所以访问 https 的时候浏览器会提醒证书不被信任。

### 安装MySQL

    $ brew install mysql

MySQL开机启动：

    $ ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
    ## 启动
    $ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist

开启MySQL安全机制。

    $ mysql_secure_installation

根据终端提示，设置密码级别，输入root密码，然后依次确认一些安全选项。

    #查看一下MySQL运行情况
    $ ps aux | grep mysql
    #测试连接MySQL
    $ mysql -uroot -p

安装 Memcache
--------------

    $ brew install memcached

启动/停止指令

    $ memcached -d
    $ killall memcached

加入开机启动

    $ ln -sfv /usr/local/opt/memcached/*.plist ~/Library/LaunchAgents
    # 启动
    $ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.memcached.plist

安装 Redis
----------

    $ brew install redis
    # 修改配置文件中 daemonize 为 yes 才能以 Deamon 方式运行
    $ vim /usr/local/etc/redis.conf
    $ redis-server /usr/local/etc/redis.conf

加入开机启动

    $ ln -sfv /usr/local/opt/redis/*.plist ~/Library/LaunchAgents
    #启动
    $ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.redis.plist

设置快捷服务控制命令
-------------------

为了后面管理方便，将命令 alias 下，`vim ~/.bash_aliases` 输入一下内容：

    alias nginx.start='launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist'
    alias nginx.stop='launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist'
    alias nginx.restart='nginx.stop && nginx.start'
    alias php-fpm.start="launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.php55.plist"
    alias php-fpm.stop="launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.php55.plist"
    alias php-fpm.restart='php-fpm.stop && php-fpm.start'
    alias mysql.start="launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist"
    alias mysql.stop="launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist"
    alias mysql.restart='mysql.stop && mysql.start'
    alias redis.start="launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.redis.plist"
    alias redis.stop="launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.redis.plist"
    alias redis.restart='redis.stop && redis.start'
    alias memcached.start="launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.memcached.plist"
    alias memcached.stop="launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.memcached.plist"
    alias memcached.restart='memcached.stop && memcached.start'

让快捷命令生效

    echo "[[ -f ~/.bash_aliases ]] && . ~/.bash_aliases" >> ~/.bash_profile
    . ~/.bash_profile

创建站点目录到主目录，方便快捷访问

    ln -sfv /var/www ~/htdocs

参考
----

- <http://segmentfault.com/a/1190000000606752>
- <http://avnpc.com/pages/install-lnmp-on-osx>
