---
layout: post
title: Javascript 中的false,零值,null,undefined和空字符串对象
date: 2012-05-29
author: scsidisk
category: IT
tags: [IT]
---

在Javascript中，我们经常会接触到题目中提到的这5个比较特别的对象--false、0、空字符串、null和undefined。这几个对象很容易用错，因此在使用时必须得小心。

### 类型检测

我们下来看看他们的类型分别是什么：

```js
alert(typeof(false) === 'boolean');
alert(typeof(0) === 'number');
alert(typeof("") === 'string');
alert(typeof(null) === 'object');
alert(typeof undefined === 'undefined');
```

运行上述代码，弹出的对话框应该显示的都是true。也就是说，false是布尔类型对象，0是数字类型对象，空字符串是字符串类型对象，null是object对象，undefined类型还是undefined。

### 互等性

当你用==操作符将false对象和其他对象进行比较的时候，你会发现， 只有0和空字符串等于false；undefined和null对象并不等于false对象，而null和undefined是相等的

```js
alert(false == undefined);
alert(false == null);
alert(false == 0);
alert(false == "");
alert(null == undefined);
```

我们可以把0、空字符串和false归为一类，称为"假值"；把null和undefined归为一类，称为"空值"。假值还算一个有效的对象，因此可以对其使用toString等类型相关的方法，而空值则不行。下面的代码将会抛出异常：

```js
alert(false.toString());    // "false"
alert("".charAt(0));        // ""
alert((0).toExponential(10));  // 0.0000000e+0
alert(undefined.toString());    // throw exception "undefined has no properties"
alert(null.toString());             // "null has no properties"
```

### 字符串表示

虽然空值不能调用toString方法，但是却可以使用String构造函数进行构造。 像decodeURI这样的函数，如果传入的是undefined或者null，返回的是"undefined"和"null"字符串 。这点很容易用错。

```js
<script type="text/javascript">
alert(String(false));    // "false"
alert(String(""));        // ""
alert(String(0));  // 0.0000000e+0
alert(String(undefined));    // "undefined"
alert(String(null));             // "null"

alert(decodeURI(undefined));// "undefined"
alert(decodeURI(null));// "null"
```

### 假值和空值作为if条件分支

假值和空值有一个共性，那就是在 作为if的条件分支时，均被视为false ；应用"!"操作之后得到的均为true 。如下示例代码：

```js
var ar = [undefined,false,0,"",null];
for(var i = 0,len = ar.length; i < len; i++){
	if(ar[i]){
	    alert("你不应该看到此对话框!");
	}
}
```

这是因为，这几个对象均被视为各自类型中的无效值或空值。因此if分支中这些对象均被视为false对待。

### null和undefined的区别

这两个空值的区别也是容易混淆的。

undefined和null对象无非是两个特殊对象，undefined表示无效对象，null表示空对象。如果变量显式或者隐式（由Javascript引擎进行赋值）地被赋予了undefined，那么代表了此变量未被定义，如果被赋予null值，则代表此变量被初始化为空值。

在Javascript中，变量是通过var声明，=赋值符进行定义（初始化变量所指向的对象）。当然，如果声明一个全局变量（作为window属性）可以不使用var关键字。变量可以在声明的同时进行定义。

```js
var undefinedVariable,nullVariable = null;
alert(undefinedVariable); // "undefined"
alert(window.undefinedVariable);        // "undefined"
alert(window.abcd);        // "undefined"
alert(nullVariable);          // "null"
alert(abcd);                    // throw exception "abcd is not defined"
```

其实， 变量如果声明了但是没有初始化，那么Javascript引擎会将此变量自动指向undefined对象。

这里需要注意，我们在上面引用window.abcd时，弹出的是undefined；而直接引用abcd变量时，却抛出了一个异常。这是由于Javascript引擎对于没有显式指定对象链的变量，会尝试从最近的作用域开始查找变量，查找失败，则退到父级作用链进行查找。如果均查找失败，则抛出"变量未定义"的异常。

这两个值在进行数字运算的时候也有不同。

```js
alert(1+undefined);    // "NaN"
alert(1+null);             // "1"
```
从null和undefined的意义上来说，这是很好理解的。