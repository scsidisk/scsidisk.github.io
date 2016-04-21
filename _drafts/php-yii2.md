PHP开发环境 - Yii2(PHP框架)
=========================

PHP框架很多，有经典的ZF，最近很受欢迎的Laravel，以高效为目标的Yaf和Phalcon。

公司喜欢功能比较全面的框架，多数会选择Yii2。和Yii相比差别较大，主要在一下几个方面。

- 命名空间(Namespace)
- 匿名函数
- 数组短语法形式： [1,2,3] 取代 array(1,2,3) 。这在多维数组、嵌套数组中，代码更清晰、简短。
- 在视图文件中使用PHP的 <?= 标签，取代 echo 语句。
- 标准PHP库(SPL) 类和接口，具体可以查看 SPL Class and Interface
- 延迟静态绑定， 具体可以查看 Late Static Bindings
- PHP标准日期时间
- 特性(Traits)
- 使用PHP intl 扩展实现国际化支持， 具体可以查看 PECL init 。

Yii2 完全地与 Composer 这一PHP包管理器整合，构建项目更加方便。

yii2 分基础版（看上去和1差不多）和高级版（分前台和后台），高级版更加适合开发大型项目。

环境
----

- Mac OS X 10.10
- PHP 5.6.10 使用 MAMP 中的 php 软链接到 `/usr/local/bin/php` 
- Composer 为全局安装

Yii2 安装
--------

下面介绍使用 Composer 在 Mac 上面进行安装。其他的安装方式参见：[Installing from an Archive File](http://www.yiiframework.com/doc-2.0/guide-start-installation.html)

> 由于Yii2.0使用了许多PHP的新特性，因此，Yii2 需要 PHP5.4.0 以上版本。

官方推荐使用Composer安装Yii。这样更方便后期维护，如果需要添加新的扩展或者升级Yii，只要一句命令就OK了。

Yii2.0要求Composer必须安装 composer asset 插件。 这个插件使得Composer可以兼容实现 [NPM](https://www.npmjs.org/) 和 [BOWER](http://bower.io/) 包管理器的功能。 NPM 和 BOWER 主要用于前端资源（如JS库等）的管理。

如果 Composer 未安装，请参考 [PHP开发环境 - Composer(包管理)]() 安装。
	
	# 对于已经安装过Composer的，可以对其进行更新
	$ composer self-update
	
	# 为Composer 安装 composer asset 插件
	$ composer global require "fxp/composer-asset-plugin:~1.1.1"

	# 使用基础模版安装 Yii2 应用到 mysite 目录下
	$ composer create-project --prefer-dist yiisoft/yii2-app-basic \
	 mysite
	
	# 使用高级模版安装，包含前台和后台在同一个项目里面
	# composer create-project --prefer-dist yiisoft/yii2-app-advanced \
	 mysite
	
如果想使用最新的开发版本的Yii基础模版，可加入 `--stability=dev` 参数。

设置密钥
-------

在 `config/web.php` 文件，`cookieValidationKey` 这个密钥主要用于cookie验证。 Composer会自动设置一个密钥，也可以自己重新设置一个密钥。

	// !!! insert a secret key in the following (if it is empty) -
	// this is required by cookie validation
	'cookieValidationKey' => 'enter your secret key here',
	
项目路径
-------

项目发布根目录为 `path/to/mysite/web`, 配置 web 服务器指向这个目录即可。

设置 rerwite 规则， apache 参考下面设置

	# Set document root to be "mysite/web"
	DocumentRoot "path/to/mysite/web"
	
	<Directory "path/to/mysite/web">
	    # use mod_rewrite for pretty URL support
	    RewriteEngine on
	    # If a directory or a file exists, use the request directly
	    RewriteCond %{REQUEST_FILENAME} !-f
	    RewriteCond %{REQUEST_FILENAME} !-d
	    # Otherwise forward the request to index.php
	    RewriteRule . index.php
	
	    # ...other settings...
	</Directory>
	
nginx配置

	server {
	    charset utf-8;
	    client_max_body_size 128M;
	
	    listen 80; ## listen for ipv4
	    #listen [::]:80 default_server ipv6only=on; ## listen for ipv6
	
	    server_name mysite.local;
	    root        /path/to/mysite/web;
	    index       index.php;
	
	    access_log  /path/to/mysite/log/access.log;
	    error_log   /path/to/mysite/log/error.log;
	
	    location / {
	        # Redirect everything that isn't a real file to index.php
	        try_files $uri $uri/ /index.php?$args;
	    }
	
	    # uncomment to avoid processing of calls to non-existing static files by Yii
	    #location ~ \.(js|css|png|jpg|gif|swf|ico|pdf|mov|fla|zip|rar)$ {
	    #    try_files $uri =404;
	    #}
	    #error_page 404 /404.html;
	
	    location ~ \.php$ {
	        include fastcgi_params;
	        fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
	        fastcgi_pass   127.0.0.1:9000;
	        #fastcgi_pass unix:/var/run/php5-fpm.sock;
	        try_files $uri =404;
	    }
	
	    location ~ /\.(ht|svn|git) {
	        deny all;
	    }
	}

配置应用环境(高级模板)
-------------------

在Yii的高级模版应用中，引入了环境的概念。环境就是指开发环境、测试环境、产品环境等。 

切换环境：

	php /path/to/mysite/init
	
如果想更加便捷，可以直接指定相关的参数：

	php /path/to/mysite/init --env=Production overwrite=All
	
环境完成
-------

到这里环境大家就完成了，访问你配置的域名，应该可以看到首页了。

在 web 目录执行
	
	$ php yii serve
	或
	$ php yii serve --port=8888
	
在浏览器里面输入 <http://localhost/> ，应该可以看到首页

常见问题
-------

### 权限管理

推荐使用 [yii2-admin](https://github.com/mdmsoft/yii2-admin) 进行权限控制。

	$ composer require mdmsoft/yii2-admin "~2.0"
	$ composer update
	
配置 `config\main.php`

	return [
	    'modules' => [
	        'admin' => [
	            'class' => 'mdm\admin\Module',
	             'layout' => 'left-menu',//yii2-admin的导航菜单
	        ]
	        ...
	    ],
	    ...
	    'components' => [
	        ...
	        'authManager' => [
	            'class' => 'yii\rbac\DbManager', // 使用数据库管理配置文件
	        ]
	    ],
	    'as access' => [
	        'class' => 'mdm\admin\components\AccessControl',
	        'allowActions' => [
	            'site/*',//允许访问的节点，可自行添加
	            'admin/*',//允许所有人访问admin节点及其子节点
	        ]
	    ],
	];

创建相应的数据库表：

	yii migrate --migrationPath=@mdm/admin/migrations
	yii migrate --migrationPath=@yii/rbac/migrations
	
访问 <http://localhost/path/to/index.php?r=admin> 可以看到yii2-admin的管理界面。

更多细节可以参阅 <http://www.yiichina.com/tutorial/571?sort=desc>

### 菜单

可以使用 Menu 进行生成菜单列表。样式可以使用 options 指定样式

	<?php
	echo Menu::widget([
	    'items' => [
	        ['label' => Yii::t('app','Dashboard'),
	            'url' => '/site/index',
	        ],
	        ['label' => Yii::t('app','Goods'),
	            'url' => '#',
	            'items' => [
	                [
		                'label' => Yii::t('app','Manage'), 
		                'url' => '/manage/index'
	                ],
	            ],
	        ],        
	    ],
	    'options' => ['class'=>'myClass'],
	]);
	?>
	
### 多机器部署静态资源找不到

主要是 AssetManager 需要验证文件的生成时间，如果不同机器上面的静态资源文件生成时间不一致，就会导致资源文件发布的时候 hash 路径不同，导致文件找不到。

可以使用自定义的 AssetManager 文件，如 MyAssetManager 继承 AssetManager。主要修改里面涉及到 filemtime 的方法。

放到 components，web.php 中添加：

	'assetManager' => [
	    'class' => '\app\components\MyAssetManager'
	],
	
### 静态资源发布分组问题

Yii2中静态资源(css,js,ttf)等资源的发布最好按照类型分组。

- AppAsset.php 放通用的资源
- ChartAsset.php 放图表展示的资源
- FormAsset.php 放表单需要的资源

按功能分组，这样在只在需要加载的时候进行加载，可以在一定程度上优化页面的显示速度。