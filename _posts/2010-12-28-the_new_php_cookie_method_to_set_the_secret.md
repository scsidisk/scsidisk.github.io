---
layout: post
title: 全新PHP Cookie设置方法揭秘
date: 2010-12-28 15:23
author: scsidisk
category: PHP
tags: [Cookie, PHP]
Slug: the_new_php_cookie_method_to_set_the_secret
---

PHP经过长时间的发展，很多用户都很了解PHP了，这里我发表一下关于PHP
Cookie设置，PHP用SetCookie函数来设置Cookie。必须注意的一点是：Cookie是HTTP协议头的一部分，用于浏览器和服务器之间传递信息，所以必须在任何属于HTML文件本身的内容输出之前调用Cookie函数。SetCookie函数定义了一个Cookie，并且把它附加在HTTP头的后面，SetCookie函数的原型如下：

<div id="lv2">
int SetCookie(string name, string value, int expire, string path, string
domain, int secure);

除了name之外所有的参数都是可选的。value,path,domain三个参数可以用空字符串代换，表示没有设置;expire
和
secure两个参数是数值型的，可以用0表示。expire参数是一个标准的Unix时间标记，可以用time()或mktime()函数取得，以秒为单位。secure参数表示这个Cookie是否通过加密的HTTPS协议在网络上传输。当前设置的Cookie不是立即生效的，而是要等到下一个页面时才能看到.这是由于在设置的这个页面里Cookie由服务器传递给客户浏览器，在下一个页面浏览器才能把Cookie从客户的机器里取出传回服务器的原因。

在同一个页面PHP
Cookie设置，实际是从后往前，所以如果要在插入一个新的Cookie之前删掉一个，你必须先写插入的语句，再写删除的语句，否则可能会出现不希望的结果。来看几个例子简单的PHP
Cookie设置：

SetCookie("MyCookie", "Value of MyCookie");

带失效时间的：

SetCookie("WithExpire", "Expire in 1 hour", time()+3600);//3600秒=1小时

什么都有的：

SetCookie("FullCookie", "Full cookie value", time()+3600, "/forum",
".phpuser.com", 1);

这里还有一点要说明的，比如你的站点有几个不同的目录，那么如果只用不带路径的Cookie的话，在一个目录下的页面里设的Cookie在另一个目录的页面里是看不到的，也就是说，Cookie是面向路径的。实际上，即使没有指定路径，WEB服务器会自动传递当前的路径给浏览器的，指定路径会强制服务器使用设置的路径。解决这个问题的办法是在调用SetCookie时加上路径和域名，域名的格式可以是“www.phpuser.com”，也可是
“.phpuser.com”。

SetCookie函数里表示value的部分，在传递时会自动被encode，也就是说，如果value的值是“test
value”在传递时就变成了“test%20value”，跟URL的方法一样。当然，对于程序来说这是透明的，因为在PHP接收Cookie的值时会自动将其decode。

如果要设置同名的多个Cookie，要用数组，方法是：

SetCookie("CookieArray[]", "Value 1"); SetCookie("CookieArray[]", "Value
2");

或

SetCookie("CookieArray[0]", "Value 1"); SetCookie("CookieArray[1]",
"Value 2");

</div>

