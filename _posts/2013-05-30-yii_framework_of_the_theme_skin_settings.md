---
layout: post
title: Yii框架的主题、皮肤设置
date: 2013-05-30
author: scsidisk
category: PHP
tags: [PHP, Yii]
---

## Theming(主题)

Theming是一个在Web应用程序里定制网页外观的系统方式。通过采用一个新的主题，网页应用程序的整体外观可以立即改变。

在Yii，每个主题由一个目录代表，包含view（视图）文件，layout（布局）文件和相关的资源文件，如images（图片）， CSS（样式）文件， JavaScript（js）文件等。主题的名字就是它的目录名字。全部主题都放在在同一目录WebRoot/themes下 。在任何时候，只有一个主题可以被激活。

要激活一个主题，设置Web应用程序的属性theme为 你所要的名字，例如：

修改应用的配置文件（protected/config/main.php）中加入

```php
<?php
return array(
	'theme'=>'basic',
);
?>
```

主题目录里面内容的组织方式和应用的根目录下的组织方式一样。例如，所有的视图文件必须位于views下 ，布局视图文件在views/layouts下 ，和系统视图文件在views/system下。例如，如果我们要替换PostController的create 视图文件为classic主题下，我们将保存新的视图文件为WebRoot/themes/classic/views/post/create.php。

当我们调用render或renderPartial显示视图，相应的view（视图）文件以及布局文件将在当前激活的主题里(themes/$themeName/views/)寻找。如果发现，这些文件将被渲染。否则，就后退到应用的视图目录下（application/views/）寻找。

在一个主题的视图内部，我们经常需要链接其他主题的资源文件。例如，我们可能要显示一个在主题下images目录里的图像文件，

方法为：yii::app()->theme->baseUrl .'/images/FileName.gif'

使用例子：

```
WebRoot/
	assets
	protected/
		.htaccess
		components/
		controllers/
		models/
		views/
			layouts/
				main.php
			site/
				index.php
	themes/
		basic/
			views/
				.htaccess
				layouts/
					main.php
				site/
					index.php
		fancy/
			views/
				.htaccess
				layouts/
					main.php
			site/
	Index.php
```

如果在应用配置（protected/config/main.php）中配置

```php
<?php
return arrray(
	'theme' => 'basic',
)
?>
```

Basic主题将生效，意味着应用的布局将使用 目录 themes/basic/views/layouts 下的视图，站点的 index 视图将使用 目录 themes/basic/views/site 下的视图。若在主题中没有找到一个视图文件，它 将后退到目录 protected/views。

## Skins(换肤)

使用皮肤来系统化的定制视图中的 widget 的外观，

为了使用皮肤特征，我们首先需要改变应用配置（protected/config/main.php）来安装 widgetFactory 组件，配置如下：

```php
<?php
return array(
	'components'=>array(
		'widgetFactory'=>array(
			'class'=>'CWidgetFactory',
		),
	),
);
?>
```

属于同一个 widget 类的皮肤被存储在 一个名字和此 widget 类相同的单个PHP脚本文件中，默认地，所有这些皮肤文件存储在目录 protected/views/skins中，当使用主题时， Yii 将也在主题的视图目录中的 skins 目录寻找皮肤。(例如WebRoot/themes/$themeName/views/skins)

例如，我们创建一个名为 CLinkPager.php 的文件到目录 protected/views/skins(或者themes/$themeName/views/skins)， 它的内容如下，

```php
<?php
return array(
	'default' => array(//
		'nextPageLabel' => '上一页',
		'prevPageLabel' => '下一页',
		'maxButtonCount' => 10,
		//'header' => '',
		'cssFile' => Yii::app()->theme->baseUrl.'/css/pager.css'
	),
	'classic' => array(
		'nextPageLabel' => '上一页classic',
		'prevPageLabel' => '下一页classic',
		//'header' => '',
		'maxButtonCount' => 5,
		'cssFile' => Yii::app()->theme->baseUrl.'/css/pager1.css',
	),
);
?>
```

在上面,我们 CLinkPager widget 创建了两个皮肤： default 和 classic。 前者是我们不明确指定 CLinkPager 的 skin属性时使用的皮肤，而后者是其skin属性被指定为 classic 时使用的皮肤。例如，在下面的视图代码中， 第一个 pager 将使用 default 皮肤而第二个使用 classic 皮肤：

```php
<?php $this->widget('zii.widgets.CListView', array(
	'summaryText' => '显示 {start}-{end} of {count} result(s).',
	'pager' => array(),
	'dataProvider'=>$dataProvider,
	'itemView'=>'_view',
)); ?>
```

```php
<?php $this->widget('zii.widgets.CListView', array(
	'summaryText' => '显示 {start}-{end} of {count} result(s).',
	'pager' => array('skin'=>'classic'),
	'dataProvider'=>$dataProvider,
	'itemView'=>'_view',
)); ?>
```
