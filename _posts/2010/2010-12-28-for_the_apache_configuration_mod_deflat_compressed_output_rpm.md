---
layout: post
title: 为Apache配置mod_deflat压缩输出[转]
date: 2010-12-28 14:55
author: scsidisk
category: Linux
tags: [Apache, mod_deflat]
Slug: for_the_apache_configuration_mod_deflat_compressed_output_rpm
---

1、如果未安装Apache。编译时，加上--enable-deflate，例如：（仅针对Linux版，Windows版无须此步骤）

```
./configure --prefix=/usr/local/apache --enable-rewrite --enable-so
--enable-deflate
```

2、如果已安装Apache。添加mod_deflate模块，例如：（仅针对Linux版，Windows版无须此步骤）

```
/usr/local/apache/bin/apxs -i -a -c
/home/zhangyan/software/httpd-2.0.59/modules/filters/mod_deflate.c
```

注：/home/zhangyan/software/httpd-2.0.59/为Apache源码路径。

3、进行以上步骤后，会在httpd.conf中自动加入一行：（Windows版请将下行最前面的#号去掉）

```
LoadModule deflate_module modules/mod_deflate.so
```

4、编辑httpd.conf，增加：

Linux版：

```
<ifmodule mod_deflate.c>
DeflateCompressionLevel 9
SetOutputFilter DEFLATE
#DeflateFilterNote Input instream
#DeflateFilterNote Output outstream
#DeflateFilterNote Ratio ratio
#LogFormat '"%r" %{outstream}n/%{instream}n (%{ratio}n%%)' deflate
#CustomLog logs/deflate_log.log deflate
</ifmodule>
```

Windows版：

```
<ifmodule deflate_module>
DeflateCompressionLevel 9
SetOutputFilter DEFLATE
#DeflateFilterNote Input instream
#DeflateFilterNote Output outstream
#DeflateFilterNote Ratio ratio
#LogFormat '"%r" %{outstream}n/%{instream}n (%{ratio}n%%)' deflate
#CustomLog logs/deflate_log.log deflate
</ifmodule>
```

如果将#号去掉，可以在logs/deflate_log.log日志文件中看到文件压缩前后的字节数、压缩比，例如：

```
"GET /index.html HTTP/1.1" 49373/276249 (17%)
```

压缩前的字节数为276249，压缩后的字节数为49373，压缩比为17%

5、详细配置见Apache官方网站：http://httpd.apache.org/docs/2.0/mod/mod_deflate.html

原文地址：http://blog.s135.com/post/293/
