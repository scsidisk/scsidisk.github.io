---
layout: post
title: 通过Alexa API获取Alexa排名
date: 2013-09-26
author: scsidisk
category: PHP
tags: [PHP]
---

通过Alexa API获取Alexa排名

我们通会用Alexa的网站(或其它站长工具网站)来栓查我们的网站流量排名，这样就必须去那些网站。实际上，可以通过Alexa XML API 获取网站的Alexa相关的数据（XML格式的），再使用XML解析器来解析Alexa返回的XML，得到Alexa排名或其它的数据。

## Alexa接口

Alexa的XML API接口是：

`http://data.alexa.com/data?cli=10&url=%YOUR_URL%`

如果想获取更多的数据可以用：

`http://data.alexa.com/data?cli=10&dat=snbamz&url=%YOUR_URL%`

用 `http://data.alexa.com/data?cli=10&dat=snbamz&url=xiangtyee.com` 返回的数据如下：

```
<ALEXA VER="0.9" URL="[xiangtyee.com/](http://xiangtyee.com/)" HOME="0" AID="ScELh1AI3f00az" IDN="[xiangtyee.com/](http://xiangtyee.com/)">
	<RLS PREFIX="http://" more="0"></RLS>
	<SD TITLE="A" FLAGS="" HOST="[xiangtyee.com](http://xiangtyee.com)">
		<LINKSIN NUM="1"/>
	</SD>
	<SD>
		<POPULARITY URL="[xiangtyee.com/](http://xiangtyee.com/)" TEXT="7552101" SOURCE="panel"/>
		<REACH RANK="6342897"/>
	</SD>
</ALEXA>
```

其中 POPULARITY 元素中的 TEXT 属性的值 7552101 就是 Alexa 排名。

## 代码实现

用 PHP 实现通过 Alexa API 获取Alexa排名的代码为：

```
function getAlexaRank ($Domain){
	$line = "";
	$data = "";
	$URL = "http://data.alexa.com/data/?cli=10&dat=snba&url=". $Domain ;
	$fp = fopen ($URL ,"r");
	if ($fp ){
		while (!feof ($fp )){
			$line = fgets ($fp );
			$data .= $line ;
		}
		$p= xml_parser_create ();
		xml_parse_into_struct ($p , $data , $vals );
		xml_parser_free ($p );
		for ($i =0 ;$i <count ($vals );$i ++){
			if ($vals [$i ]["tag"]##"POPULARITY"){
				return $vals [$i ]["attributes"]["TEXT"];
			}
		}
	}
}
?>
```

```
<?php
echo getAlexaRank("[xiangtyee.com](http://xiangtyee.com)");
?>
```

## 参考

- http://code.google.com/p/seostats
- http://tutology.net/category/how-php/get-alexa-rank-php-and-alexa-api
