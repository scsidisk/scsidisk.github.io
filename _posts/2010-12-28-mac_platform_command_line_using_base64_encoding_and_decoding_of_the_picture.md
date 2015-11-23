---
layout: post
title: Mac平台命令行下使用base64对图片编码和解码
date: 2010-12-28 13:51
author: scsidisk
category: MacOSX
tags: [Mac, Shell]
Slug: mac_platform_command_line_using_base64_encoding_and_decoding_of_the_picture
---

解码:

openssl base64 -d -in \<infile\> -out \<outfile\>

编码:

openssl base64 -in \<infile\> -out \<outfile\>

网页中引用：

\<image src="data:image/gif;base64,XXXXX.....XXXXXXX"\>

\<image src="data:image;base64,XXXXX.....XXXXXXX"\>

<div class="posttagsblock">
</div>

