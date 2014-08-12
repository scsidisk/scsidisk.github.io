---
layout: post
title: YII autoload 与第三方冲突
date: 2014-08-12
author: scsidisk
category: PHP
tags: PHP Yii
---

YII autoload 可能与第三方的类库中的 autoload 冲突，下面以 PHPExcel 为例说明。

处理PHPExcel autoload和YII autoload相冲突的方法任选其一，推荐第4种，最符合YII标准）

## 1、修改PHPExcel中的Autoloader.php

```
PHPExcel_Autoloader::Register();  
PHPExcel_Shared_ZipStreamWrapper::register();  
```

修改为

```
Yii::registerAutoloader(array('PHPExcel_Autoloader','Register'),true);  
```

## 2、修改Autoloader.php文件中的Register方法

```
public static function Register() {  
    /* 
    if (function_exists('__autoload')) { 
        //Register any existing autoloader function with SPL, so we don't get any clashes 
        spl_autoload_register('__autoload'); 
    } 
    //Register ourselves with SPL 
    return spl_autoload_register(array('PHPExcel_Autoloader', 'Load')); 
    */  
    $functions = spl_autoload_functions();  
    foreach ( $functions as  $function)  
        spl_autoload_unregister($function);  
    $functions = array_merge(array(array('PHPExcel_Autoloader','Load')),$functions);  
    foreach ( $functions as $function)  
        $x = spl_autoload_register($function);  
    return $x;  
}    //    function Register()  
```

## 3、在需要使用进行设置

```
$filePath = '/home/public_html/sqt/protected/data/queueSql/company.xls';  
spl_autoload_unregister(array('YiiBase', 'autoload'));  
$phpExcelPath = Yii::getPathOfAlias('application.extensions.PHPExcel.PHPExcel');  
include($phpExcelPath . DIRECTORY_SEPARATOR . 'IOFactory.php');  
spl_autoload_register(array('YiiBase', 'autoload'));  
$PHPExcel = PHPExcel_IOFactory::load( $filePath);  
```

## 4、设置enableIncludePath

```
Yii::$enableIncludePath = false;    
Yii::import('application.vendors.phpexcel.PHPExcel', 1);  
```

转自：http://blog.csdn.net/tinico/article/details/18033575