---
layout: post
title: Mac下面Nginx+Lua+Redis整合实现高性能API接口
date: 2013-01-25
author: scsidisk
category: Development
tags: [Lua, Redis, Nginx, Mac]
---

准备对陌陌的部分API进行加速，考虑使用Nginx+Lua实现

### 安装Lua

从http://www.lua.org/download.html下载

```
$ wget http://www.lua.org/ftp/lua-5.1.5.tar.gz
$ tar zxf lua-5.1.5.tar.gz
$ cd lua-5.1.5
$ make macosx
$ sudo make install
```

下面是输出的lua安装信息

```
cd src && mkdir -p /usr/local/bin /usr/local/include /usr/local/lib \
/usr/local/man/man1 /usr/local/share/lua/5.1 /usr/local/lib/lua/5.1
cd src && install -p -m 0755 lua luac /usr/local/bin
cd src && install -p -m 0644 lua.h luaconf.h lualib.h lauxlib.h \
../etc/lua.hpp /usr/local/include
cd src && install -p -m 0644 liblua.a /usr/local/lib
cd doc && install -p -m 0644 lua.1 luac.1 /usr/local/man/man1
```

来个hello momo吧，安装完成后输入lua进入Lua控制台

```
$ lua
lua > print 'hello, momo!'
```

### 下载 Nginx 及相关模块

安装Lua 5.1后，继续尝试Nginx lua模块的安装，同时安装redis2-nginx-module、set-misc-nginx-module等实现api请求到达nginx，然后lua查询redis，redis有数据直接返回，否则请求php接口填充，填充完成后继续从redis返回JSON数据。

```
$ wget http://nginx.org/download/nginx-nginx-1.2.6.tar.gz
$ tar zxvf nginx-nginx-1.2.6.tar.gz
```

### 下载NDK、lua模块、redis模块，基本来自淘宝

```
$ git clone https://github.com/simpl/ngx_devel_kit.git
$ git clone https://github.com/chaoslawful/lua-nginx-module.git
$ git clone https://github.com/agentzh/redis2-nginx-module.git
$ git clone https://github.com/agentzh/set-misc-nginx-module.git
$ git clone https://github.com/agentzh/echo-nginx-module.git
```

### 安装支持库

lua-redis-parser为本机编译链接库，不是nginx module

```
$ git clone https://github.com/agentzh/lua-redis-parser.git
```

编译方法详见http://wiki.nginx.org/LuaRedisParser，如果是Mac OS X系统

```
$ make LDFLAGS='-bundle -undefined dynamic_lookup' CC=gcc
$ sudo make install

$ wget http://www.kyne.com.au/~mark/software/download/lua-cjson-2.0.0.tar.gz
```

在Mac下编译

```
$ make LDFLAGS='-bundle -undefined dynamic_lookup'
$ sudo make install
```

### 编译nginx

```
$ ./configure --prefix=/usr/local/nginx --add-module=../ngx_devel_kit/ \
--add-module=../lua-nginx-module --add-module=../redis2-nginx-module \
--add-module=../set-misc-nginx-module --add-module=../echo-nginx-module \
--with-ld-opt="-L /usr/local/Cellar/pcre/8.31/lib"
$ make
$ sudo make install

$ vim nginx.conf
```

修改端口为8099,也可以不改

### 启动nginx

```
$ sudo /usr/local/nginx/sbin/nginx
$ curl -I http://localhost:8099
```

### 编写第一个lua模块

修改相应内容如下

nginx.conf

```nginx
server {
 listen 8099;
 server_name localhost;

 location /lua_hello {
  default_type 'text/plain';
  content_by_lua "ngx.say('Hello, Nginx Lua!')";
 }

 # GET /redis_get?key=your_key
 location /redis_get {
  set_unescape_uri $key $arg_key;
  redis2_query get $key;
  redis2_pass 127.0.0.1:6379;
 }

 # GET /redis_set?key=your_key&value=your_value
 location /redis_set {
  set_unescape_uri $key $arg_key;
  set_unescape_uri $val $arg_val;
  redis2_query set $key $val;
  redis2_pass 127.0.0.1:6379;
 }

 location /redis {
  internal;
  set_unescape_uri $query $arg_query;
  redis2_raw_query $query;
  redis2_pass 127.0.0.1:6379;
 }

 location /lua_redis {
  content_by_lua_file conf/momo.lua;
 }
}
```

conf/momo.lua:

```lua
ngx.say('Hello from file!')
```

其中 localhost 可以改成本地域名并调整本地hosts

```
$ sudo /usr/local/nginx/sbin/nginx -t
$ sudo /usr/local/nginx/sbin/nginx -s reload
$ curl http://localhost:8099/lua_hello
... Hello, Nginx Lua!
$ curl 'http://localhost:8099/redis_set?key=name&val=latermoon'
$ curl 'http://localhost:8099/redis_get?key=name'
$ curl 'http://localhost:8099/lua_redis'
```
