---
layout: post
title: dooPHP带表名前缀的表生成模型为单独文件
date: 2011-02-13 15:07
author: scsidisk
category: PHP
tags: [DooPHP, PHP]
Slug: doophp_gen_model_singel_file
---

修改 dooframework/db/DooModelGen.php

----

public static function gen\_mysql(\$comments=true, \$vrules=true,
\$extends='DooModel', \$createBase=true, \$baseSuffix='Base',
\$chmod=null) {

self::genMySQL(\$comments, \$vrules, \$extends, \$createBase,
\$baseSuffix, \$chmod);

...

public static function genMySQL(\$comments=true, \$vrules=true,
\$extends='DooModel', \$createBase=true, \$baseSuffix='Base',
\$chmod=null, \$tablePrefix=null) {

...

\$classname = '';

if(\$tablePrefix){

\$temptbl = str\_replace(\$tablePrefix,'',\$tblname);

}else{

\$temptbl = \$tblname;

}

----

为

----

public static function gen\_mysql(\$comments=true, \$vrules=true,
\$extends='DooModel', \$createBase=true, \$baseSuffix='Base',
\$chmod=null, \$tablePrefix=null) {

self::genMySQL(\$comments, \$vrules, \$extends, \$createBase,
\$baseSuffix, \$chmod, \$tablePrefix);

...

public static function genMySQL(\$comments=true, \$vrules=true,
\$extends='DooModel', \$createBase=true, \$baseSuffix='Base',
\$chmod=null, \$tablePrefix=null) {

...

\$classname = '';

if(\$tablePrefix){

\$temptbl = str\_replace(\$tablePrefix,'',\$tblname);

}else{

\$temptbl = \$tblname;

}

----

修改控制器 MainController.php

----

public function gen\_model(){

Doo::loadCore('db/DooModelGen');

DooModelGen::genMySQL(true,true, 'DooModel', false, 'Base', null);

}

----

为

----

public function gen\_model(){

Doo::loadCore('db/DooModelGen');

DooModelGen::genMySQL(true,true, 'DooModel', false, 'Base', null,
'tb\_');

}

----

... 为省略的部分，自己搜索一下。
重新生成模型即可。不生成 Base 目录，直接生成单独的 model
文件，如果想生成 Base ， 把 第四个 false 改成true 即可。
