---
layout: post
title: php获取/检查当前类名方法名
date: 2013-03-22 18:46
author: scsidisk
category: PHP
tags: [PHP, Class, Function]
---

### 1. 使用函数检查

- function_exists() - Return TRUE if the given function has been defined
- is_callable() - 检测参数是否为合法的可调用结构
- class_exists() - 检查类是否已定义
- method_exists() - 检查类的方法是否存在

### 2. 使用魔方常量获取相关名称

- \_\_FUNCTION\_\_ 函数被定义时的名字（区分大小写）。在 PHP 4 中该值总是小写字母的。(PHP 4.3.0+)
- \_\_CLASS\_\_	 类被定义时的名字（区分大小写）。在 PHP 4 中该值总是小写字母的。类名包括其被声明的作用区域（例如 Foo\Bar）。注意自 PHP 5.4 对 trait - 也起作用。是调用 trait 方法的类的名字。(PHP 4.3.0+)
- \_\_METHOD\_\_	 方法被定义时的名字（区分大小写）。(PHP 5.0.0+)

其他有用的魔法常量

- \_\_LINE\_\_	 文件中的当前行号。
- \_\_FILE\_\_	 文件的完整路径和文件名。如果在被包含文件中，则返回被包含的文件名。自 PHP 4.0.2 - 起，总是绝对路径（如果是符号连接，则解析），之前是可能是相对路径。
- \_\_DIR\_\_	 文件所在的目录。如果用在被包括文件中，则返回被包括的文件所在的目录。它等价于 dirname(\_\_FILE\_\_)。除非是根目录，否则目录中名不包括末尾的斜杠。（PHP 5.3.0+）
- \_\_TRAIT\_\_	 trait 被定义时的名字（区分大小写）。Trait 名包括其被声明的作用区域（例如 Foo\Bar）。(PHP 5.4.0+)
- \_\_NAMESPACE\_\_	 当前命名空间的名称（区分大小写）。此常量是在编译时定义的（PHP 5.3.0+）。

注：这些常量前后均是两个下划线。

### 3. 使用函数获取相关名称

- get\_class(class name); //取得当前语句所在类的类名
- get\_class\_methods(class name); //取得class name 类的所有的方法名，并且组成一个数组
- get\_class\_vars(class name); //取得class name 类的所有的变亮名，并组成一个数组
