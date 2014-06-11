---
layout: post
title: Sphider + SCWS 打造完美PHP中文搜索引擎
date: 2012-10-19
author: scsidisk
category: PHP
tags: Sphider, SCWS, PHP
---

今日需要为几个网站做个全文搜索引擎，找了几个PHP开源项目，先试了一下Sphinx ，可惜是基于数据库的，相当于数据库搜索的扩展。Sphider还不错，不过中文的分词不行，基本只能靠空格和符号进行分词。想用luence的话只能用Java和.net了，没有php版的，因此只好尝试自己修改Sphider的分词了。还好找到了SCWS这个不错的中文分词系统，只需要把他的功能加入到Sphider里面就可以了。

先按照他们的安装文档部署好Sphider和SCWS，这里使用的SCWS-1.1.6，需要部署好PHP扩展，注意Linux下要修改词库的权限，否则分词会把所有汉字单独分开。Sphider这里使用的丁廷臣简体中文完美汉化版带蜘蛛搜索引擎。

两者部署无误后，修改Sphider，找到admin文件夹下的spider文件，首先在开始加入代码初始化分词程序

注意这里使用的gbk，如果你的网页用的utf8编码，要把这里以及词典和规则文件的位置更改一下

在index\_url函数中，把原有的英文分词替换掉，在$wordarray = unique_array(explode(" ", $data['content']));前面加上

```php
<?php
$cws->send_text($data['content']);
$list = $cws->get_tops(1000, $xattr);
settype($list, 'array');
$wordarray = array();
$i=0;
//segment
foreach($list as $tmp)
{
	$wordarray[$i][1] = $tmp['word'];
	$wordarray[$i][2] = $tmp['times'];
	$i++;
}
```
 
删除

```php
<?php
$wordarray = unique_array(explode(" ", $data['content']));
```

和

```php
<?php
$wordarray = calc_weights ($wordarray, $title, $host, $path, $data['keywords']);
```

两个语句，因为Sphider原有的英文分词这里就完全没必要用了，这里可以自行对$wordarray进行限制和优化，这里我写的很简单。

修改完成后，爬虫就能正常对中文进行分词了，效果还不错，注意如果出现乱码注意网页或者辞典编码是utf8还是gb2312。
