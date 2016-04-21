PHP开发环境 - Composer(包管理)
============================

Composer 是 PHP 的一个包依赖管理工具。它允许你申明项目所依赖的代码库。

官方网站：<https://getcomposer.org/>

官方文档：<https://getcomposer.org/doc/>

中文网站：<http://www.phpcomposer.com/>

国内镜像：<http://pkg.phpcomposer.com/>


环境
----

- Mac OS X 10.10
- PHP 5.6.10 使用 MAMP 中的 php 软链接到 `/usr/local/bin/php`
- Composer 为全局安装


系统要求
-------

运行 Composer 需要 ***PHP 5.3.2+*** 以上版本。

将从包的来源直接安装，而不是简单的下载 zip 文件，你需要 git 、 svn 或者 hg ，这取决于你载入的包所使用的版本管理系统。

Composer 支持 Windows 、 Linux 以及 OSX。

全局安装
-------

Windows上面, 你需要下载并执行 [Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe).

	$ curl -sS https://getcomposer.org/installer | php
	$ mv composer.phar /usr/local/bin/composer

或者使用国内镜像

	$ curl -sS http://install.phpcomposer.com/installer | \
	sudo php -- --install-dir=/usr/local/bin --filename=composer

> 注意： 需要权限， 请使用 sudo 命令。
>
> 如果执行 composer 没有反映，查一下 /usr/local/bin/ 在 $PATH 中的顺序

如果你愿意通过 brew 安装，可以参考下面步骤

	$ brew update
	$ brew tap josegonzalez/homebrew-php
	$ brew tap homebrew/versions
	$ brew install php55-intl
	$ brew install josegonzalez/php/composer

查看版本信息

	$ composer -V

composer.json
-------------

要开始在你的项目中使用 Composer，你只需要一个 composer.json 文件。该文件包含了项目的依赖和其它的一些元数据。

### 关于 require Key

你需要简单的告诉 Composer 你的项目需要依赖哪些包。

	{
	    "require": {
	        "monolog/monolog": "1.0.*"
	    }
	}

你可以看到， require 需要一个 包名称 （例如 `monolog/monolog`） 映射到 `包版本` （例如 `1.0.*`） 的对象。

### 版本约束

可以用几个不同的方法来指定。

- **指定版本** (如：`1.0.2`) 你可以指定包的确切版本。
- **范围**	(如：`>=1.0` `>=1.0,<2.0` `>=1.0,<1.1|>=1.2`) 可以指定范围。`>`、`>=`、`<`、`<=`、`!=`。 逗号隔开，视为AND。管道隔开`|`视为OR。AND 的优先级高于 OR。
- **通配符** (如：`1.0.*`) 相当于 `>=1.0,<1.1` 。
- **赋值运算符** (如：`~1.2`) 相当于 `>=1.2,<2.0` 。

> 默认情况下只有稳定的发行版才会被考虑在内。如果你也想获得 RC、beta、alpha 或 dev 版本，你可以使用 `稳定标志`。你可以对所有的包做 `最小稳定性` 设置，而不是每个依赖逐一设置。

安装依赖包
--------

获取定义的依赖到你的本地项目。

	$ composer install

将 `monolog/monolog` 下载到 vendor 目录。将创建 vendor/monolog/monolog 目录。

> 小技巧： 如果你正在使用Git来管理你的项目， 你可能要添加 vendor 到你的 .gitignore 文件中。 你不会希望将所有的代码都添加到你的版本库中。

composer.lock - 锁文件
---------------------

在安装依赖后，Composer 将把安装时的版本号列表写入 composer.lock 文件。这将锁定改项目的特定版本。

> 请提交你应用程序的 composer.lock （包括 composer.json）到你的版本库中

Composer 会检查锁文件是否存在，如果存在，它将下载指定的版本（忽略 composer.json 文件中的定义）。

如果要更新你的依赖版本请使用 update 命令。

	$ composer update

如果只想安装或更新一个依赖。

	$ composer update monolog/monolog [...]

自动加载
-------

Composer 生成了一个 `vendor/autoload.php` 文件。你要引入这个文件。

	require 'vendor/autoload.php';

你可以开始使用这个类库，并且他们将被自动加载。

	$log = new Monolog\Logger('name');
	$log->pushHandler(new Monolog\Handler\StreamHandler(
			'app.log',
			Monolog\Logger::WARNING
		));
	$log->addWarning('Foo');

你可以在 composer.json 的 autoload 字段中增加自己的 autoloader。

	{
	    "autoload": {
	        "psr-4": {"Acme\\": "src/"}
	    }
	}

这里定义一个从命名空间到目录的映射。此时 `src` 会在你项目的根目录，与 `vendor` 文件夹同级。例如 `src/Foo.php` 文件应该包含 `Acme\Foo` 类。

重新运行 `install` 命令来生成 `vendor/autoload.php` 文件。

引用这个文件也将返回 autoloader 的实例，你可以将包含调用的返回值存储在变量中，并添加更多的命名空间。这对于在一个测试套件中自动加载类文件是非常有用的，例如。

	$loader = require 'vendor/autoload.php';
	$loader->add('Acme\\Test\\', __DIR__);

除了 `PSR-4` 自动加载，`classmap` 也是支持的。这允许类被自动加载，即使不符合 `PSR-0` 规范。

> 注意： Composer 提供了自己的 autoloader。如果你不想使用它，你可以仅仅引入 vendor/composer/autoload_*.php 文件，它返回一个关联数组，你可以通过这个关联数组配置自己的 autoloader。

常用命令
-------

	$ composer -v ## 输出反馈信息
	$ composer -vv ## 输出更详细的反馈信息
	$ composer -vvv ## 输出debug信息
	$ composer -h ## 显示帮助
	$ composer -V ## 显示版本
	$ composer init ## 初始化 composer.json 文件
	$ composer require ## 增加依赖包到 composer.json
	$ composer require vendor/package:2.* ## 直接指明依赖包
	$ composer require vendor/package2:dev-master ## 直接指明依赖包
	$ composer global require fabpot/php-cs-fixer:dev-master ## 安装全局命令行
	$ composer global update ## 更新全局命令行
	$ composer install ## 安装依赖
	$ composer install --prefer-dist ## 尽可能从 dist 获取，
		将大幅度的加快安装进度。
	$ composer install -o ## 转换 PSR-0/4 autoloading 到 classmap
		可以获得更快的加载支持
	$ composer update [包] ## 更新包
	$ composer require 包 ## 添加依赖
	$ composer search 包 ## 搜索包
	$ composer show 包 ## 显示包信息
	$ composer depends --link-type=require monolog/monolog ## 检查依赖关系
	$ composer validate ## 验证 composer.json 文件是否有效
	$ composer self-update ## 更新 composer

Packagist / Composer 中国全量镜像
-------------------------------

修改 composer 的全局配置文件（推荐方式）

	$ composer config -g repo.packagist composer \
		http://packagist.phpcomposer.com
