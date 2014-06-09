---
layout: post
title: 在WordPress首页和目录页显示摘要的方法
date: 2010-12-28 14:54
author: scsidisk
category: Other
Slug: home_and_directories_in_wordpress_page_shows_summary_methods
---

在WordPress系统中，默认的首页和目录页使用的书全文输出，这对于文章内容较长的博客来说很不方面，下面我介绍一个方法，可以简单的实现在WordPress首页和目录页显示摘要而非全文。

首先找到wp-content/themes下你使用的模板目录，查找目录中的文件，如果有home.php则修改home.php，没有的话就修改index.php，找到\<?php
the\_content(); ?\>这一行，将其修改为以下代码：

\<?php if(is\_category() || is\_archive() || is\_home() ) {

the\_excerpt();

} else {

the\_content('Read the rest of this entry &raquo;');

} ?\>

\<div class="details"\>\<div class="inside"\>\<?php
comments\_popup\_link('No Comments', '1 Comment', '% Comments'); ?\> so
far | \<a href="\<?php the\_permalink() ?\>"\>Read On
&raquo;\</a\>\</div\>\</div\>

这时，你的WordPress首页和分类就显示为摘要信息而不是全文信息了。

这段代码可以在你的首页、存档页、目录页使用摘要输出，使用摘要输出后，整个WordPress的重复内容就少多了，很利于搜索引擎优化。
