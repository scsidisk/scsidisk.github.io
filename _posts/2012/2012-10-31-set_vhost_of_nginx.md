---
layout: post
title: "Nginx下虚拟主机环境变量的配置方法"
date: 2012-10-31
author: scsidisk
category: Linux
tags: [Nginx]
---

将在项目中使用的需要与服务器交互的参数用虚拟主机的环境变量来统一进行管理，这是我在新浪学到的第一个服务器管理技巧。这么做的好处是保证了项目代码的完全可移植性和并行开发时的一致性，避免了在本地开发提交代码时频繁地修改连接服务器参数。好的东西就一定要加以学习和推广，这也是我的口号"Do after learning"。于是我就想到了这个博客的服务器。

本博客的服务器采用Nginx 0.7.62，因此，设置虚拟主机的环境变量是通过配置fastcgi_param来实现的。以下就本机实际操作举例说明：

为了方便统筹管理，我将每个虚拟主机的配置都单独用一个文件来进行管理，放在Nginx虚拟主机文件所在目录下(如/usr/local/nginx/conf/vhosts/)，文件名为该虚拟主机的域名。比如本博客的虚拟主机文件为:/usr/local/nginx/conf/vhosts/blog.lewsen.com。同样的思路，我们也可以将环境变量的配置也单独成文件，然后在虚拟主机配置中include进去即可。示例步骤如下：

1、在Nginx虚拟主机文件所在目录下(如/usr/local/nginx/conf/vhosts/)，新建一个名为"fastcgi_param_blog.lewsen.com"的文件，配置内容如下：

 ```nginx
# PHP only, required if PHP was built with --enable-force-cgi-redirect

# for example
fastcgi_param      SRV_KONGKONG             "hello, kongkong!";

# fastcgi_param_blog.lewsen.com
fastcgi_param       SRV_SERVER_NAME "blog.lewsen.com";
fastcgi_param       SRV_DOCUMENT_ROOT   "/data/www/wwwroot/blog.lewsen.com/";
fastcgi_param       SRV_SERVER_PORT "80";

fastcgi_param       SRV_CACHE_DIR   "/data/www/cache/blog.lewsen.com/";
fastcgi_param       SRV_DATA_DIR    "/data/www/data/blog.lewsen.com/";

fastcgi_param       SRV_DB_HOST     "127.0.0.1";
fastcgi_param       SRV_DB_NAME     "lewsen";
fastcgi_param       SRV_DB_PASS     "password";
fastcgi_param       SRV_DB_PORT     "3306";
fastcgi_param       SRV_DB_USER     "lewsen";

fastcgi_param       SRV_DB_HOST_R       "127.0.0.1";
fastcgi_param       SRV_DB_NAME_R       "lewsen";
fastcgi_param       SRV_DB_PASS_R       "password";
fastcgi_param       SRV_DB_PORT_R       "3306";
fastcgi_param       SRV_DB_USER_R       "lewsen_r";

fastcgi_param       SRV_MEMCACHED_HOST      "127.0.0.1";
fastcgi_param       SRV_MEMCACHED_KEY_PREFIX        "lewsen-";
fastcgi_param       SRV_MEMCACHED_PORT      "11211";
fastcgi_param       SRV_MEMCACHED_SERVERS       "127.0.0.1:11211";
```

2、包含"fastcgi_param_blog.lewsen.com"。
打开虚拟主机配置文件：

```
 vi /usr/local/nginx/conf/vhosts/blog.lewsen.com
```

在include fastcgi_params;这行下面添加一行：

```nginx
include fastcgi_params;
include fastcgi_param_blog.lewsen.com;
```

3、重载Nginx的配置

```
service nginx reload
```

Done!

至此，Nginx下虚拟主机环境变量的配置就已经完成。这时候，我们就可以在该虚拟主机下的PHP应用中直接调用这些变量，比如：

```php
<?php
echo $_SERVER['SRV_KONGKONG'];  //hello, kongkong!
?>
```

转自：张宴的博客