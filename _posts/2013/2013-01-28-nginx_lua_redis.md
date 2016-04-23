---
layout: post
title: "使用 Lua 脚本语言操作 Redis"
date: 2013-01-28
author: scsidisk
categories: Development
tags: Lua, Redis, Nginx
---

使用 Lua 脚本语言操作 Redis。

要使用 content_by_lua_file，需要安装 nginx_lua_module 模块。

大神 章亦春 提供了一个很方便的操作redis的开发包，如下：

```
git clone https://github.com/agentzh/lua-resty-redis.git
```

该包中，有一个 Lib 目录，将 Lib 目录下的文件和子目录拷贝至目录 /data/www/lua
在 Nginx 配置文件中，需要加一行代码，以便引入 redis.lua。
注：加在 http 段里。

```nginx
lua_package_path "/data/www/lua/?.lua;;";
```

为了使得 lua 脚本的修改能及时生效，需要加入一行代码，如下：
注：在 server 段里，加入代码，如果不加此代码或者设置为 on 时，则需要重启 Nginx。

```nginx
lua_code_cache off;
```

在 Nginx 配置文件中，加入一个Location：

```nginx
location /lua {
    content_by_lua_file /data/www/lua/test.lua;
}
```

注：引入 test.lua 脚本文件

Lua 脚本文件：test.lua。

```lua
local redis = require "resty.redis"
local cache = redis.new()
local ok, err = cache.connect(cache, '127.0.0.1', '6379')

cache:set_timeout(60000)

if not ok then
        ngx.say("failed to connect:", err)
        return
end

res, err = cache:set("dog", "an aniaml")
if not ok then
        ngx.say("failed to set dog: ", err)
        return
end

ngx.say("set result: ", res)

local res, err = cache:get("dog")
if not res then
        ngx.say("failed to get dog: ", err)
        return
end

if res == ngx.null then
        ngx.say("dog not found.")
        return
end

ngx.say("dog: ", res)

local ok, err = cache:close()

if not ok then
        ngx.say("failed to close:", err)
        return
end
```

测试结果如下：

```
$ curl http://localhost/lua
set result: OK
dog: an aniaml
```