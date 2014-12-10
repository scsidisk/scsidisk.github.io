---
layout: post
title: Yii: Change jQuery Version and add JQuery-UI
date: 2014-09-05
author: Erkan Buelbuel
category: PHP
tags: Yii
---

Just edit

```
/protected/config/main.php
```

Define an alias before the return-array :

```
Yii::setPathOfAlias('themeDir','/v1.2/themes/bootstrap/js/jquery'); 
return array(
...
```

Than add the clienScript-package in the components-part:

```
    'components'=>array(
...
        'clientScript'=>array(
            'packages'=>array(
                'jquery'=>array(
                    'baseUrl'=>Yii::getPathOfAlias('themeDir'),
                    'js'=>array('jquery-1.11.1.min.js')
                ),
                'jquery-ui'=>array(
                    'baseUrl'=>Yii::getPathOfAlias('themeDir'),
                    'js'=>array('jquery-ui-1.10.4.min.js')
                )
            )
        ),
...
```

and activate it in the view:

```php
<?php Yii::app()->clientScript->registerCoreScript('jquery-ui'); ?>
```
The standard package array structure is as follows:


array(
  'package-name'=>array(
    'basePath'=>'alias of the directory containing the script files',
    'baseUrl'=>'base URL for the script files',
    'js'=>array(list of js files relative to basePath/baseUrl),
    'css'=>array(list of css files relative to basePath/baseUrl),
    'depends'=>array(list of dependent packages),
  ),
  ......
)
 
See more details -> here
This entry was posted in Frameworks, PHP, Web Development and tagged CClientScript, clientScript, jquery, jquery-UI, yii.
