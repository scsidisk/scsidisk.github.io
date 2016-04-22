---
layout: post
title: "源代码文档生成 Doxygen介绍"
date: 2012-10-31
author: scsidisk
category: MacOSX
tags: [Doxygen]
---

## 一、Doxygen介绍

在项目开发过程中最重要的是如何和团队中其它成员沟通，如何在项目完成后减低维护成本，随着公司的人员流动，怎样使新员工快速熟悉以前的项目？解决这些问题最重要的资料就是文档和代码，但是如果文档和代码不同步，则会适得其反。Doxygen能够很好的解决代码和文档同步问题，在代码中加入有@标识符的注释，修改代码时实时修改注释，然后通过Doxygen可以生成更新后的文档。

Doxygen是基于GPL的开源项目，是一个非常优秀的文档系统，当前支持在大多数unix（包括linux），windows家族，Mac系统上运行，完全支持C++, C, Java, IDL（Corba和Microsoft 家族）语言，部分支持PHP和C#语言，输出格式包括HTML、latex、RTF、ps、PDF、压缩的HTML和unix manpage，Doxygen软件可以从这里下载，软件本身用法非常简单。

## 二、Doxygen安装

### 1、在Windows里面安装和使用

Windows下安装包可以在https://olex.openlogic.com/packages/doxygen#1894下载安装，最新版本为1.5.6。

第一步，配置Doxygen，选择Wizard，填写源代码目录，生成目标文件目录

选择All entities，c++ output

选择prpare for compressed HTML

第二步，保存配置文件

第三步，选择目录

第四步，点击Start，开始生成文件。

在完成后，可以在目标目录下可以看到目录html，在该目录下点击index.html，就可以浏览生成后的文档，如果想要生成单一的chm文件，下载chm制作精灵来制作。

### 2、在Carbide c++中安装

Carbide c++ 中使用doxygen：打开Help -> Software Updates -> Find and Install... -> Search for new features to install -> Next -> New Remote Site...，

url输入http://download.gna.org/eclox/update，如下图1所示，然后选择Finish，等下载完成后，选择安装，安装完后重启carbide c++，这样可以在carbide c++工具栏中找到@图形。

配置Doxygen更新

## 三、Doxygen语法

### 1. 模块定义（单独显示一页）

```
/*
 * @defgroup 模块名 模块的说明文字
 * @{
 */

 ... 定义的内容 ...

/** @} */ // 模块结尾
```

### 2. 分组定义（在一页内分组显示）

```
/*
 * @name 分组说明文字
 * @{
 */

 ... 定义的内容 ...

/** @} */
```

### 3. 变量、宏定义、类型定义简要说明

```
/** 简要说明文字 */
#define FLOAT float

/** @brief 简要说明文字（在前面加 @brief 是标准格式） */
#define MIN_UINT 0

/*
 * 分行的简要说明 \n
 *  这是第二行的简要说明
 */
int b;
```

### 4. 函数说明

```
/*
 * 简要的函数说明文字
 *  @param [in] param1 参数1说明
 *  @param [out] param2 参数2说明
 *  @return 返回值说明
 */
int func(int param1, int param2);

/*
 * 打开文件 \n
 *  文件打开成功后，必须使用 ::CloseFile 函数关闭。
 *  @param[in] file_name 文件名字符串
 *  @param[in] file_mode 文件打开模式字符串，可以由以下几个模块组合而成：
 *  - r 读取
 *  - w 可写
 *  - a 添加
 *  - t 文本模式(不能与 b 联用)
 *  - b 二进制模式(不能与 t 联用)
 *  @return 返回文件编号
 *  - -1 表示打开文件失败

 *  @note 文件打开成功后，必须使用 ::CloseFile 函数关闭
 *  @par 示例:
 *  @code
    // 用文本只读方式打开文件
    int f = OpenFile("d:\\test.txt", "rt");
 *  @endcode

 *  @see ::ReadFile ::WriteFile ::CloseFile
 *  @deprecated 由于特殊的原因，这个函数可能会在将来的版本中取消。
 */
int OpenFile(const char* file_name, const char* file_mode);
```

### 5. 枚举类型定义

```
/** 枚举常量 */
typedef enum TDayOfWeek
{
SUN = 0, /**<  星期天（注意，要以 “<” 小于号开头） */
MON = 1, /**<  星期一 */
TUE = 2, /**<  星期二 */
WED = 3, /**<  星期三 */
THU = 4, /**<  星期四 */
FRI = 5, /**<  星期五 */
SAT = 6  /**<  星期六 */
}
/** 定义类型 TEnumDayOfWeek */
TEnumDayOfWeek;
```
　　
### 6. 项目符号标记

```
  /*
   *  A list of events:
   *    - mouse events
   *         -# mouse move event
   *         -# mouse click event\n
   *            More info about the click event.
   *         -# mouse double click event
   *    - keyboard events
   *         -# key down event
   *         -# key up event
   *
   *  More text here.
   */
```

结果为：

```
A list of events:

mouse events
mouse move event
mouse click event
More info about the click event.
mouse double click event
keyboard events
key down event
key up event
More text here.
```

代码示范：

```c++
/*
 * @defgroup EXAMPLES 自动注释文档范例
 * @author  沐枫
 * @version 1.0
 * @date    2004-2005
 * @{
 */


/*
 * @name 文件名常量
 * @{
 */

/** 日志文件名 */
#define LOG_FILENAME "d:\\log\\debug.log"
/** 数据文件名 */
#define DATA_FILENAME "d:\\data\\detail.dat"
/** 存档文件名 */
#define BAK_FILENAME "d:\\data\\backup.dat"

/** @}*/ // 文件名常量

/*
 * @name 系统状态常量
 *  @{
 */

/** 正常状态 */
#define SYS_NORMAL 0
/** 故障状态 */
#define SYS_FAULT 1
/** 警告状态 */
#define SYS_WARNNING 2

/** @}*/ // 系统状态常量

/** 枚举常量 */
typedef enum TDayOfWeek
{
  SUN = 0, /**< 星期天 */
  MON = 1, /**< 星期一 */
  TUE = 2, /**< 星期二 */
  WED = 3, /**< 星期三 */
  THU = 4, /**< 星期四 */
  FRI = 5, /**< 星期五 */
  SAT = 6  /**< 星期六 */
}
/** 定义类型 TEnumDayOfWeek */
TEnumDayOfWeek;
/** 定义类型 PEnumDayOfWeek */
typedef TEnumDayOfWeek* PEnumDayOfWeek;

/** 定义枚举变量 enum1 */
TEnumDayOfWeek enum1;
/** 定义枚举指针变量 enum2 */
PEnumDayOfWeek p_enum2;



/*
 * @defgroup FileUtils 文件操作函数
 * @{
 */

/*
 * 打开文件 \n
 *  文件打开成功后，必须使用 ::CloseFile 函数关闭。
 *  @param[in] file_name 文件名字符串
 *  @param[in] file_mode 文件打开模式字符串，可以由以下几个模块组合而成：
 *  - r 读取
 *  - w 可写
 *  - a 添加
 *  - t 文本模式(不能与 b 联用)
 *  - b 二进制模式(不能与 t 联用)
 *  @return 返回文件编号
 *  - -1 表示打开文件失败

 *  @note 文件打开成功后，必须使用 ::CloseFile 函数关闭
 *  @par 示例:
 *  @code
    // 用文本只读方式打开文件
    int f = OpenFile("d:\\test.txt", "rt");
 *  @endcode

 *  @see ::ReadFile ::WriteFile ::CloseFile
 *  @deprecated 由于特殊的原因，这个函数可能会在将来的版本中取消。
 */
int OpenFile(const char* file_name, const char* file_mode);

/*
 * 读取文件
 *  @param[in] file 文件编号，参见：::OpenFile
 *  @param[out] buffer 用于存放读取的文件内容
 *  @param[in] len 需要读取的文件长度
 *  @return 返回读取文件的长度
 *  - -1 表示读取文件失败

 *  @pre \e file 变量必须使用 ::OpenFile 返回值
 *  @pre \e buffer 不能为 NULL
 *  @see ::OpenFile ::WriteFile ::CloseFile
 */
int ReadFile(int file, char* buffer, int len);

/*
 * 写入文件
 *  @param[in] file 文件编号，参见：::OpenFile
 *  @param[in] buffer 用于存放将要写入的文件内容
 *  @param[in] len 需要写入的文件长度
 *  @return 返回写入的长度
 *  - -1 表示写入文件失败

 *  @pre \e file 变量必须使用 ::OpenFile 返回值
 *  @see ::OpenFile ::ReadFile ::CloseFile
 */
int WriteFile(int file, const char* buffer, int len);

/*
 * 关闭文件
 *  @param file 文件编号，参见：::OpenFile
 *  @retval 0  为成功
 *  @retval -1 表示失败

 *  @see ::OpenFile ::WriteFile ::ReadFile
 *  @deprecated 由于特殊的原因，这个函数可能会在将来的版本中取消。
 */
int CloseFile(int file);

/** @}*/ // 文件操作函数

/** @}*/ // 自动注释文档范例
```

