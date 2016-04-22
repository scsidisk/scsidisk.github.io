---
layout: post
title: Yii安装rights
date: 2012-10-19
author: scsidisk
category: PHP
tags: [Sphider, SCWS, PHP]
---

## 1. 安装rights

复制rights到modules

## 2. 修改main.php

```php
<?php
'import'=>array(
     ......
     'application.modules.rights.*',
     'application.modules.rights.components.*', // Correct paths if necessary.
),
......
'components'=>array(
     ......
     'user'=>array(
          'class'=>'RWebUser', // Allows super users access implicitly.
          ......
     ),
     'authManager'=>array(
          'class'=>'RDbAuthManager', // Provides support authorization item
sorting.
          ......
     ),
     ......
),
'modules'=>array(
     'rights'=>array(
          'install'=>true, // Enables the installer.
     ),
),
```

## 2. 修改controller

```php
<?php
class UserDataMonitoringController extends RController

public function filters()
{
     return array(
          'rights', // perform access control for CRUD operations
     );
}
```

## 3. 删除

```php
<?php
public function accessRules()
......
```
