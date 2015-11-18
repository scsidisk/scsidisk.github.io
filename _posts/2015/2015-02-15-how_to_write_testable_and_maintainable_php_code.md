---
layout: post
title: 如何编写可测试和可维护的php代码
date: 2015-02-15
author: scsidisk
category: PHP
tags: [PHP, PHPUnit]
---



============================



框架提供了一个工具可以快速的开发应用，框架允许你快速的创建功能，但是通常你会获得技术债务。当可维护性不是作为开发人员的主要目的，技术债务就产生了。由于缺少单元测试和构架，未来的修改和调试就变得非常昂贵了。

这里我们将讨论如何构建可测试和可维护的代码。

本文会涉及到：

1.  DRY
2.  依赖注入
3.  接口
4.  容器
5.  phpunit单元测试

让我们从一段故意设计，但是非常典型的代码开始。在很多框架中，这可能是一个模型类:

```php
<?php
class User
{
    public function getCurrentUser()
    {
        $user_id = $_SESSION['user_id'];
        $user = App::db->select('id, username')
            ->where('id', $user_id)
            ->limit(1)
            ->get();
        if ($user->num_results() > 0) {
            return $user->row();
        }
        return false;
    }
}
```

这段代码虽然可以工作，但是需要改进:

1.  这段代码不具备可测试性。我们依赖\$\_SESSION全局变量，像phpunit这类测试框架是在命令行模式下运行的，\$\_SESSION和其它一些全局变量可能并不会生效。

2.  这段代码的可维护性不是很好。例如：我们修改了数据源，应用中每一处使用了App::db实例的地方都需要修改。此外，如果我们不仅仅是需要当前用户信息（需要获取别的用户的信息）怎么办呢？

一次单元测试的尝试
------------------

这里我们为上面的功能尝试创建一个单元测试。

```php
<?php
class UserModelTest extends PHPUnit_Framework_TestCase
{
    public function testGetUser()
    {
        $user = new User();
        $currentUser = $user->getCurrentUser();
        $this->assertEquals(1, $currentUser->id);
    }
}
```

让我们检查下，首先，这个测试会失败。在User对象中使用的\$\_SESSION 并不存在于单元测试中，因为是在命令行运行的php。

其次，这里没有设置数据库连接，这意味着，想让这个测试单元工作，我们需要启动应用来获取App对象和它的db对象，我们还需要一个数据库连接来进行测试。

为了让这个单元测试工作，我们需要：

1. 在我们的应用中为 命令行界面（PHPUnit）设置配置文件
2. 依赖一个数据库连接，这样做意味着我们依靠一个独立于单元测试的数据源，如果我们的测试数据库没有期望的数据会怎样？如果我们的数据库连接很慢会怎样？
3. 依赖一个已经启动的应用会增加测试的负担，会明显降低单元测试的速度。理想情况下，我们的大部分的可被测试的代码独立于我们所使用的框架。

那么，让我们着手考虑如何改善这些问题。

保持代码DRY(don’t repeat yourself)
----------------------------------

在这个简单的环境中检索当前用户信息的功能是不必要的，这是一个人为设计的例子，但本着DRY原则精神，我首先选择先优化“getUser”方法，使其更通用。

```php
<?php
class User
{
    public function getUser($user_id)
    {
        $user = App::db->select('user')
            ->where('id', $user_id)
            ->limit(1)
            ->get();
        if ($user->num_results() > 0) {
            return $user->row();
        }
        return false;
    }
}
```

现在"getUser"方法可以在整个应用中使用了。我们可以通过用户id来调用到当前用户，而不是把这个功能（返回当前用户信息）封装到模型里面。当代码不再依赖其它功能（比如：session全局变量）时，就变得模块化和可维护。

但是，这依然没有实现代码本应有的可测试性和可维护性，我们仍然依赖数据库连接。

依赖注入
--------

我们通过引入“依赖注入”来改善情况。我们的模型类看起来可能是这样的，当我们传入数据库连接到这个类：

```php
<?php
class User
{

    protected $_db;

    public function __construct($db_connection)
    {
        $this->_db = $db_connection;
    }

    public function getUser($user_id)
    {
        $user = $this->_db->select('user')
            ->where('id', $user_id)
            ->limit(1)
            ->get();

        if ($user->num_results() > 0) {
            return $user->row();
        }

        return false;
    }

}
```

现在，我们User模型的依赖被提供，我们的类不再使用一个确定的数据库连接，也不依赖全局变量。

从这点讲，我们的类基本上可测试的。我们可以传递我们选择的数据源和用户id，然后测试调用的结果。我们同样可以切换到不同的数据库连接（假设两者都实现了相同的数据检索方法），酷。

让我们看看单元测试可能的样子：

```php
<?php

use Mockery as m;
use Fideloper\User;

class SecondUserTest extends PHPUnit_Framework_TestCase
{

    public function testGetCurrentUserMock()
    {
        $db_connection = $this->_mockDb();

        $user = new User($db_connection);

        $result = $user->getUser(1);

        $expected = new StdClass();
        $expected->id = 1;
        $expected->username = 'fideloper';

        $this->assertEquals($result->id, $expected->id, 'User ID set correctly');
        $this->assertEquals($result->username, $expected->username, 'Username set correctly');
    }

    protected function _mockDb()
    {
        // "Mock" (stub) database row result object
        $returnResult = new StdClass();
        $returnResult->id = 1;
        $returnResult->username = 'fideloper';

        // Mock database result object
        $result = m::mock('DbResult');
        $result->shouldReceive('num_results')->once()->andReturn(1);
        $result->shouldReceive('row')->once()->andReturn($returnResult);

        // Mock database connection object
        $db = m::mock('DbConnection');

        $db->shouldReceive('select')->once()->andReturn($db);
        $db->shouldReceive('where')->once()->andReturn($db);
        $db->shouldReceive('limit')->once()->andReturn($db);
        $db->shouldReceive('get')->once()->andReturn($result);

        return $db;
    }

}
```

我在单元测试中加入了新东西：Mockery，Mockery让你可以模拟php对象。在这个例子中，我们模拟了数据连接，使用模拟，我们可以跳过测试数据连接，从而简化模型测试。

想了解[Mockery](http://code.tutsplus.com/tutorials/mockery-a-better-way--net-28097)？

这个例子中，我们模拟了一个sql连接。我们告诉模拟对象它有select、where、limit、和get方法会被调用。我返回模拟对象本身，sql连接对象返回它本身（\$this），这就是所谓的“链式”方法。注意，对于get方法，我返回的是数据库调用结果——一个用户数据填充的stdClass对象。

这样解决了几个问题：

1.  我们只测试我们的模型类，不需要同时测试数据库链接。

2.  我们可以控制数据库连接对象的输入和输出，因此，我们可以对数据库的调用结果进行可靠的测试。我知道我会获得userid为1的数据库调用结果。

3.  我们不需要为测试启动我们的整个应用，也不需要任何配置和数据库。

我们仍然可以做得更好，这里会变得很有趣。

接口
----

为了进一步改进，我们定义和实现了一个接口，考虑下面的代码：

```php
<?php
interface UserRepositoryInterface
{
    public function getUser($user_id);
}

class MysqlUserRepository implements UserRepositoryInterface
{

    protected $_db;

    public function __construct($db_conn)
    {
        $this->_db = $db_conn;
    }

    public function getUser($user_id)
    {
        $user = $this->_db->select('user')
            ->where('id', $user_id)
            ->limit(1)
            ->get();

        if ($user->num_results() > 0) {
            return $user->row();
        }

        return false;
    }

}

class User
{

    protected $userStore;

    public function __construct(UserRepositoryInterface $user)
    {
        $this->userStore = $user;
    }

    public function getUser($user_id)
    {
        return $this->userStore->getUser($user_id);
    }

}
```

这里我们做了几件事：

1.  首先，我们为user数据源定义了一个接口。其中定义了getUser方法。

2.  接下来，我们实现了这个接口。这个例子中，我们创建了mysql实现，我们接收一个数据库连接对象，通过它来从数据库抓取一个用户。

3.  最后我们强制，user模型类中使用的类必须实现UserInterface。这样保证了数据源始终有getUser方法，不管实现UserInterface时使用了哪种数据源。

记住我们的user对象的类型提示“UserInterface”是在它的构造函数，这意味着一个实现UserInterface的类必须被传递到User对象，这样保证了我们需要的getUser方法始终是有效的。

这样做的结果是什么？

-   我们的代码现在是完全可测试的，对于User类，我们可以简单的模拟数据源（数据源的测试将会是独立的单元测试工作）。

-   我们的代码有很好的可维护性。我们可以切换不同的数据源，而不用到处改变应用的代码。

-   我们可以创建任何的数据源：ArrayUser, MongoDbUser, CouchDbUser,
    MemoryUser等。

-   我们可以根据需要传递任意数据源到User对象。如果你决定丢弃Mysql，你只需要创建一个不同的实现（比如：MongoDbUser），把它传递给User模型。

同样，我们简化了单元测试。

```php
<?php
 
use Mockery as m;
use Fideloper\User;
 
class ThirdUserTest extends PHPUnit_Framework_TestCase {
 
    public function testGetCurrentUserMock()
    {
        $userRepo = $this->_mockUserRepo();
 
        $user = new User( $userRepo );
 
        $result = $user->getUser( 1 );
 
        $expected = new StdClass();
        $expected->id = 1;
        $expected->username = 'fideloper';
 
        $this->assertEquals( $result->id, $expected->id, 'User ID set correctly' );
        $this->assertEquals( $result->username, $expected->username, 'Username set correctly' );
    }
 
    protected function _mockUserRepo()
    {
        // Mock expected result
        $result = new StdClass();
        $result->id = 1;
        $result->username = 'fideloper';
 
        // Mock any user repository
        $userRepo = m::mock('Fideloper\Third\Repository\UserRepositoryInterface');
        $userRepo->shouldReceive('getUser')->once()->andReturn( $result );
 
        return $userRepo;
    }
 
}
```

我们完全省去了数据库连接的工作，现在我们很容易的模拟数据源，并且告诉它当getUser被调用时应该做什么？

但是，我们可以做得更好！

容器
----

考虑下我们当前代码的用法：

```php
<?php
// In some controller
$user = new User( new MysqlUser( App:db->getConnection("mysql") ) );
$user_id = App::session("user->id");

$currentUser = $user->getUser($user_id);
```

我们的最后一步是介绍容器。在上面的代码中，我们需要使用一堆对象，仅仅只是为了获取我们的当前用户。这些代码可能会散落在你的整个应用中。如果你需要从mysql切换到MongoDB，你仍然需要修改以上代码出现的每个地方。这几乎不可能DRY。容器可以解决这个问题。

一个容器仅仅包含一个对象或功能。它很像你应用中的注册表。我们可以通过容器（使用所有需要的依赖）自动初始化一个User对象。下面，我使用了[Pimple](http://pimple.sensiolabs.org/)，一个流行的容器类。

```php
<?php
// Somewhere in a configuration file
$container = new Pimple();
$container["user"] = function() {
    return new User( new MysqlUser( App:db->getConnection('mysql') ) );
}
 
// Now, in all of our controllers, we can simply write:
$currentUser = $container['user']->getUser( App::session('user_id') );
```

我把创建User对象移动到了应用中的某个地方，结果是：

1.  我们保持我们的代码DRY，User对象和数据源的选择在应用中的一个地方定义。
2.  我们修改User对象使用的Mysql修改成其它数据源时，只在一个地方修改。这样极大的提高了可维护性。

总结
----

在本教程中，我们实现了以下几点：

1.  保持代码DRY和可重复使用
2.  创建可维护的代码——如果我们需要，我们可以在一个地方为整个应用切换数据源。
3.  使我们的代码可测试——我们可以简单的模拟对象，而不需要启动整个应用和创建测试数据库。
4.  为了创建可测试和可维护代码，学习了使用依赖注入和接口。
5.  看见了容器是如何帮助我们的应用更具可维护性

我敢肯定，你注意到了，我们以可维护性和可测试性的名义增加了更多的代码。反对这种实施一个强有力的依据是：我们增加了复杂性。确实，这需要项目参与者更深入的了解代码。

但是我们的付出，完全被技术债务抵消掉了：

-   代码获得极大的可维护性，在一个地方修改变成了可能，而不是在多个地方修改。
-   单元测试能够（快速）大幅度的减少代码中的bug，尤其是在那些长期或社区主导型（开源）的项目中。
-   预先做些额外的工作，可以节约以后的时间和避免头疼。

资源
----

你可以使用[Composer](http://getcomposer.org/)方便的在你的项目中引入phpunit和Mockery。composer.json文件中的”require-dev”项：

```yaml
"require-dev": {
    "mockery/mockery": "0.8.*",
    "phpunit/phpunit": "3.7.*"
}
```

安装

```php
<?php
$ php composer.phar install --dev
```

很多PHP框架（比如laravel）也用到了容器和上面写到的一些其它概念。

英文原文：[How to Write Testable and Maintainable Code in
PHP](http://code.tutsplus.com/tutorials/how-to-write-testable-and-maintainable-code-in-php--net-31726)

转自：http://www.tangmanong.com/article/2.html


