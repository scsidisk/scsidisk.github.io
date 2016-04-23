---
layout: post
title: "$_COOKIE 默认保存时间"
date: 2012-12-28 15:23
author: scsidisk
categories: PHP
tags: Cookie, PHP
---

刚才有同学在群里询问：$_COOKIE 的时间是多长，他指的是“我直接用
$_COOKIE存取的”，也就是说用$_COOKIE这个全局变量保存一个值。那么这个值会存在多长时间，而不是用setcookie来指定。

那么这个值到底是保存多长时间呢？在PHP手册上面查询，没有找到结果，最后发现是在php.ini里指定的。

```
; Lifetime in seconds of cookie or, if 0, until browser is restarted.
session.cookie\_lifetime = 0

; The path for which the cookie is valid.
session.cookie\_path = /

; The domain for which the cookie is valid.
session.cookie\_domain =

; Whether or not to add the httpOnly flag to the cookie, which makes it
inaccessible to browser scripting languages such as JavaScript.
session.cookie\_httponly =
```

php.ini里面可以设置session.cookie\_lifetime这个值，即默认cookie保存多少秒，如果为0的话那么就和浏览器进程是相同的。

```
<?php
$coo = 'xxx';
$_COOKIE['xxx'] = $coo;
var_dump($_COOKIE);
?>
```

结果为 array(2) { ["ZDEDebuggerPresent"]=\> string(14) “php,phtml,php3″
["xxx"]=\> string(3) “xxx” }

而如果我把代码改为如下内容

```
<?php
var_dump($_COOKIE);
?>
```

刷新浏览器，结果为：array(1) { ["ZDEDebuggerPresent"]=\> string(14)
“php,phtml,php3″ }

$_COOKIE默认的值由php.ini中的session.cookie_lifetime指定。
