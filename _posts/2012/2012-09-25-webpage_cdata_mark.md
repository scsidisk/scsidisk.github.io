---
layout: post
title: 网页中 CDATA 标记
date: 2012-09-25
author: scsidisk
category: HTML
tags: HTML
---

经常在网页html代码中看见这样的嵌入标签，但实际使用没有用过，特此在记录下：

CDATA是在XML文档里面使用的关键字，用来告诉浏览器，这部分内容不用解析，是给其他程序用的，比如JAVASCRIPT等等。

PCDATA是在 XML约束文档里使用的，如DTD类型的约束文档，在这里面表示元素的内容或属性的取值范围等等，是字符串形式的。

```js
<script>
<![CDATA[
function matchwo(a,b){
if (a < b && a < 0){
     return 1;
  }else{
     return 0;
  }
}
]]>
</script>
```

常常嵌入在script标签中，用于js使用。在javaeye博客中已有详细介绍，具体就不在说明了。

参考：

http://mxdxm.iteye.com/blog/738334