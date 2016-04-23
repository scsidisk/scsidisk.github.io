---
layout: post
title: "Sublime run Java code after compiling"
date: 2012-12-12
author: scsidisk
categories: MacOSX
tags: Java, Sublime
---


### 1. 创建执行脚本

```
$ vi /usr/lcoal/bin/javacr
```

```bash
#!/bin/sh
file=$1
file_dir=`dirname ${file}`
file_name=`basename ${file}`
cd ${file_dir}

if [ ! -f "${file_name%.*}.class" ]; then
        rm -f ${file_name%.*}.class
fi

echo "Compiling ${file_name} ......."
javac ${file_name%.*}.java

echo "----------- OUTPUT ----------- "
java ${file_name%.*}
```

```
$ chmod u+x /usr/lcoal/bin/javacr
```

### 2.  sublime text 2 设置

Preferences--browse packages--打开java 文件夹--编辑Java.sublime-build

```
{
     "cmd": ["javacr", "$file"],
     "file_regex": "^(...*?):([0-9]*):?([0-9]*)",
     "selector": "source.java",
     "encoding": "cp936"
}
```

### 3. 测试

```
$ cd ~/Desktop/
$ vi Demo.java
```

```java
public class Demo{
    public static void main(String[] args){
        System.out.println("This is my test program.");
        int a = 10;
        int b = 20;
        int c = a + b;
        System.out.println("Result : " + c);
    }
}
```

使用sublime text2 打开Demo.java

按 command+b 应该看到调试结果

```
Compiling Demo.java.......
-----------OUTPUT-----------
This is my test program.
Result : 30
[Finished in 0.8s]
```


