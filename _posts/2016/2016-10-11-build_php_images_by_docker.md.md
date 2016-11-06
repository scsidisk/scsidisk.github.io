---
layout: post
title: "Docker在PHP项目开发中的应用"
date: 2016-10-11
author: scsidisk
category: Virtualization
tags: Docker, PHP
---

在 PHP 的开发过程, 线上环境和测试环境都是运维来维护, 本地环境一般是程序员在本地机器搭建开发环境.

怎样做到本地环境和线上的环境配置一致, 不会出现本地运行良好的程序到测试环境和线上环境不能运行的问题.

环境依赖的服务比较多, 可能会用到：

-   Web服务器：Nginx
-   Web程序：PHP + Node
-   数据库：MySQL
-   搜索引擎：ElasticSearch
-   队列服务：Gearman
-   缓存服务：Redis + Memcache
-   前端构建工具：npm + bower + gulp
-   PHP CLI工具：Composer + PHPUnit

这么多的服务怎么样方便的进行部署, 使用 Docker 可以更加方便的解决上面的问题.

本文中假设你的操作系统为Linux, 已经安装了Docker, 并且已经了解[Docker是什么](https://www.docker.com/whatisdocker/), 以及[Docker命令行的基础使用](https://docs.docker.com/userguide/), 如果没有这些背景知识建议先自行了解.

容器规划
---------

创建 Docker 容器可以把所有服务放到一个容器里面, 将所有服务放在一个容器内的模式有个形象的非官方称呼：Fat Container.

与之相对的是将服务分拆到容器的模式. 从Docker的设计可以看到, 构建镜像的过程中可以指定唯一一个容器启动的指令, 因此Docker天然适合一个容器只运行一种服务, 而这也是官方更推崇的.

分拆服务遇到的第一个问题就是, 我们每一个服务的基础镜像从哪里来？这里有两个选项：

- 继承官方**OS镜像**, 自己制作**服务镜像**( Nginx 和 MySQL 等)

    这种方式的优点在于所有服务可以有一个统一的基础镜像, 对镜像进行扩展和修改时可以使用同样的方式, 比如选择了ubuntu, 就可以使用`apt-get`指令安装服务.

    问题在于大量的服务需要自己维护, 特别是有时候需要某个服务的不同版本时, 往往需要直接编译源码, 调试维护成本都很高.

- 继承官方**服务镜像**( Nginx 和 MySQL 等)

    [Docker
    Hub](https://registry.hub.docker.com/)上官方已经准备好了大量常用的[服务镜像](https://registry.hub.docker.com/repos/library/?s=stars), 同时也有非常多第三方提交的镜像.

    自己也可以基于[Docker-Registry](https://github.com/docker/docker-registry)项目搭建一个私有的Docker
    Hub.

    这种方式可以以很小的代价切换服务的版本. 问题在于官方镜像的构建方式多种多样, 进行扩展时需要先了解原镜像的`Dockerfile`.

出于让服务搭建更灵活的考虑, 我们选择后者构建镜像.

容器创建
---------

为了分拆服务, 现在我们的目录变为如下所示结构：

    ~/Dockerfiles
    ├── mysql
    │   └── Dockerfile
    ├── nginx
    │   ├── Dockerfile
    │   ├── nginx.conf
    │   └── sites-enabled
    │       ├── default.conf
    │       └── evaengine.conf
    ├── php
    │   ├── Dockerfile
    │   ├── composer.phar
    │   ├── php-fpm.conf
    │   ├── php.ini
    │   ├── redis.tgz
    ├── redis
    │   └── Dockerfile
    └── docker-compose.yml

即为每个服务创建单独文件夹, 并在每个服务文件夹下放一个Dockerfile.

### MySQL容器

MySQL继承自官方的[MySQL5.6镜像](https://registry.hub.docker.com/_/mysql), Dockerfile仅有一行, 无需做任何额外处理, 因为普通需求官方都已经在镜像中实现了, 因此Dockerfile的内容为：

    FROM mysql:5.6

在项目根目录下运行

    docker build -t eva/mysql ./mysql

运行的时候需要挂载本地目录到`/var/lib/mysql`用于存放数据库, 同时要求必须通过环境变量设置一个管理员密码, 因此可以使用以下指令运行容器：

    docker run -p 3306:3306 -v ~/opt/data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -it eva/mysql

通过上面的指令, 我们将本地的3306端口绑定到容器的3306端口, 将容器内的数据库持久化到本地的`~/opt/data/mysql`, 并且为MySQL设置了一个root密码`123456`

### Nginx容器

由于官方的[Nginx1.9](https://registry.hub.docker.com/_/nginx/)是基于Debian Jessie的, 因此首先将准备好的配置文件复制到指定位置, 替换镜像内的配置, 这里按照个人习惯, 约定`/opt/htdocs`目录为Web服务器根目录, `/opt/log/nginx`目录为Nginx的Log目录.

Nginx目录下提前准备了Nginx配置文件`nginx.conf`以及项目的配置文件`default.conf`等. Dockerfile内容为：

    FROM nginx:1.9

    ADD  nginx.conf      /etc/nginx/nginx.conf
    ADD  sites-enabled/*    /etc/nginx/conf.d/
    RUN  mkdir /opt/htdocs && mkdir /opt/log && mkdir /opt/log/nginx
    RUN  chown -R www-data.www-data /opt/htdocs /opt/log

    VOLUME ["/opt"]

构建镜像

    docker build -t eva/nginx ./nginx

运行容器

    docker run -p 80:80 -v ~/opt:/opt -it eva/nginx

注意我们将本地的80端口绑定到容器的80端口, 并将本地的`~/opt`目录挂载到容器的`/opt`目录, 这样就可以将项目源代码放在`~/opt`目录下并通过容器访问了.

### PHP容器

PHP容器是最复杂的一个, 因为在实际项目中, 我们很可能需要单独安装一些PHP扩展, 并用到一些命令行工具, 这里我们以Redis扩展以及Composer来举例. 首先将项目需要的扩展等文件提前下载到php目录下, 这样构建时就可以从本地复制而无需每次通过网络下载, 大大加快镜像构建的速度：

    wget https://getcomposer.org/composer.phar -O php/composer.phar
    wget https://pecl.php.net/get/redis-2.2.7.tgz -O php/redis.tgz

php目录下还准备好了php配置文件`php.ini`以及`php-fpm.conf`, 基础镜像我们选择的是[PHP
5.6-FPM](https://registry.hub.docker.com/_/php/), 这同样是一个Debian
Jessie镜像. 官方比较亲切的在镜像内部准备了一个`docker-php-ext-install`指令, 可以快速安装如GD、PDO等常用扩展. 所有支持的扩展名称可以通过在容器内运行`docker-php-ext-install`获得.

来看一下Dockerfile

    FROM php:5.6-fpm

    ADD php.ini    /usr/local/etc/php/php.ini
    ADD php-fpm.conf    /usr/local/etc/php-fpm.conf

    COPY redis.tgz /home/redis.tgz
    RUN docker-php-ext-install gd \
        && docker-php-ext-install pdo_mysql \
        && pecl install /home/redis.tgz && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini
    ADD composer.phar /usr/local/bin/composer
    RUN chmod 755 /usr/local/bin/composer

    WORKDIR /opt
    RUN usermod -u 1000 www-data

    VOLUME ["/opt"]

按照个人习惯, 仍然设置`/opt`目录作为工作目录.

这里有一个细节, 在复制tar包文件时, 使用的Docker指令是`COPY`而不是`ADD`, 这是由于`ADD`指令会[自动解压`tar`文件](https://docs.docker.com/reference/builder/#add).

现在终于可以构建+运行了：

    docker build -t eva/php ./php
    docker run -p 9000:9000 -v ~/opt:/opt -it eva/php

在大多数情况下, Nginx和PHP所读取的项目源代码都是同一份, 因此这里同样挂载本地的`~/opt`目录, 并且绑定9000端口.

### PHP-CLI的实现

php容器除了运行php-fpm外, 还应该作为项目的php cli使用, 这样才能保证php版本、扩展以及配置文件保持一致.

例如在容器内运行Composer, 可以通过下面的指令实现：

    docker run -v $(pwd -P):/opt -it eva/php composer install --dev -vvv

这样在任意目录下运行这行指令, 等于动态将当前目录挂载到容器的默认工作目录并运行, 这也是PHP容器指定工作目录为`/opt`的原因.

同理还可以实现phpunit、npm、gulp等命令行工具在容器内运行.

如果在已经运行的容器中执行命令可以使用下面的形式

    docker exec -it eva/php [path to/]php --version

### Redis容器

为了方便演示, Redis仅仅作为缓存使用, 没有持久化需求, 因此Dockerfile仅有一行

    FROM redis:3.0

容器的连接
----------

上面已经将原本在一个容器中运行的服务分拆到多个容器, 每个容器只运行单一服务. 这样一来容器之间需要能互相通信. Docker容器间通讯的方法有两种, 一种是像上文这样将容器端口绑定到一个本地端口, 通过端口通讯. 另一种则是通过Docker提供的[Linking功能](https://docs.docker.com/userguide/dockerlinks/), 在开发环境下, 通过Linking通信更加灵活, 也能避免端口占用引起的一些问题, 比如可以通过下面的方式将Nginx和PHP链接起来：

    docker run -p 9000:9000 -v ~/opt:/opt --name php -it eva/php
    docker run -p 80:80 -v ~/opt:/opt -it --link php:php eva/nginx

在一般的PHP项目中, Nginx需要链接PHP, 而PHP又需要链接MySQL, Redis等. 为了让容器间互相链接更加容易管理, Docker官方推荐使用[Docker-Compose](https://docs.docker.com/compose/)完成这些操作.

用一行指令完成安装

    pip install -U docker-compose

然后在Docker项目的根目录下准备一个docker-compose.yml文件, 内容为：

    nginx:
        build: ./nginx
        ports:
          - "80:80"
        links:
          - "php"
        volumes:
          - ~/opt:/opt

    php:
        build: ./php
        ports:
          - "9000:9000"
        links:
          - "mysql"
          - "redis"
        volumes:
          - ~/opt:/opt

    mysql:
        build: ./mysql
        ports:
          - "3306:3306"
        volumes:
          - ~/opt/data/mysql:/var/lib/mysql
        environment:
          MYSQL_ROOT_PASSWORD: 123456

    redis:
        build: ./redis
        ports:
          - "6379:6379"

然后运行`docker-compose up`, 就完成了所有的端口绑定、挂载、链接操作.

更复杂的实例
------------

上面是一个标准PHP项目在Docker环境下的演进过程, 实际项目中一般会集成更多更复杂的服务, 但上述基本步骤仍然可以通用. 比如[EvaEngine/Dockerfiles](https://github.com/EvaEngine/Dockerfiles)是为了运行我的开源项目[EvaEngine](http://avnpc.com/pages/eva-engine)准备的基于Docker的开发环境, EvaEngine依赖了队列服务Gearman, 缓存服务Memcache、Redis, 前端构建工具Gulp、Bower, 后端Cli工具Composer、PHPUnit等. 具体实现方式可以自行阅读代码.

经过团队实践, 原本大概需要1天时间的环境安装, 切换到Docker后只需要运行10余条指令, 时间也大幅缩短到3小时以内（大部分时间是在等待下载）, 最重要的是Docker所构建的环境都是100%一致的, 不会有人为失误引起的问题. 未来我们会进一步将Docker应用到CI以及生产环境中.

转载：http://avnpc.com/pages/build-php-develop-env-by-docker