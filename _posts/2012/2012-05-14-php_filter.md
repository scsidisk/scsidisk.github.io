---
layout: post
title: "PHP Filter 简介"
date: 2012-05-14
author: scsidisk
categories: PHP
tags: PHP, Filter
---

PHP 过滤器用于对来自非安全来源的数据（比如用户输入）进行验证和过滤。

```
filter_has_var()     检查是否存在指定输入类型的变量。
filter_id()     返回指定过滤器的 ID 号。
filter_input()     从脚本外部获取输入，并进行过滤。
filter_input_array()     从脚本外部获取多项输入，并进行过滤。
filter_list()     返回包含所有得到支持的过滤器的一个数组。
filter_var_array()     获取多项变量，并进行过滤。
filter_var()     获取一个变量，并进行过滤。。
```

```
FILTER_CALLBACK     调用用户自定义函数来过滤数据。
FILTER_SANITIZE_STRING     去除标签，去除或编码特殊字符。
FILTER_SANITIZE_STRIPPED     "string" 过滤器的别名。
FILTER_SANITIZE_ENCODED     URL-encode 字符串，去除或编码特殊字符。
FILTER_SANITIZE_SPECIAL_CHARS     HTML 转义字符 '"<>& 以及 ASCII 值小于 32 的字符。
FILTER_SANITIZE_EMAIL     删除所有字符，除了字母、数字以及 !#$%&'*+-/=?^_`{|}~@.[]
FILTER_SANITIZE_URL     删除所有字符，除了字母、数字以及 $-_.+!*'(),{}|\\^~[]`<>#%";/?:@&=
FILTER_SANITIZE_NUMBER_INT     删除所有字符，除了数字和 +-
FILTER_SANITIZE_NUMBER_FLOAT     删除所有字符，除了数字、+- 以及 .,eE。
FILTER_SANITIZE_MAGIC_QUOTES     应用 addslashes()。
FILTER_UNSAFE_RAW     不进行任何过滤，去除或编码特殊字符。
FILTER_VALIDATE_INT     在指定的范围以整数验证值。
FILTER_VALIDATE_BOOLEAN     如果是 "1", "true", "on" 以及 "yes"，则返回 true，
						如果是 "0", "false", "off", "no" 以及 ""，则返回 false。否则返回 NULL。
FILTER_VALIDATE_FLOAT     以浮点数验证值。
FILTER_VALIDATE_REGEXP     根据 regexp，兼容 Perl 的正则表达式来验证值。
FILTER_VALIDATE_URL     把值作为 URL 来验证。
FILTER_VALIDATE_EMAIL     把值作为 e-mail 来验证。
FILTER_VALIDATE_IP     把值作为 IP 地址来验证
```

### 语法

```
filter_input(input_type, variable, filter, options)
```

| 参数       |    描述                                      |
| ---------- | --------------------------------------------|
| input_type |    必需。规定输入类型。参见上面的列表中可能的类型。|
| variable   |  规定要过滤的变量。|
| filter     |  可选。规定要使用的过滤器的 ID。默认是 FILTER_SANITIZE_STRING。<br /> 请参见完整的 PHP Filter 函数参考手册，获得可能的过滤器。<br /> 过滤器 ID 可以是 ID 名称 （比如 FILTER_VALIDATE_EMAIL），<br /> 或 ID 号（比如 274）。|
| options    | 规定包含标志/选项的数组。检查每个过滤器可能的标志和选项。 |

### 例子

在本例中，我们使用 filter_input() 函数来过滤一个 POST 变量。所接受的 POST 变量是合法的 e-mail 地址。

```php
<?php
if (!filter_input(INPUT_POST, 'email', FILTER_VALIDATE_EMAIL)) {
    echo "E-Mail is not valid";
} else {
    echo "E-Mail is valid";
}
?>
```