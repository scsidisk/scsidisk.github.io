---
layout: post
title: "Yii中使用RBAC完全指南"
date: 2013-06-07
author: scsidisk
categories: PHP
tags: PHP, Yii
---

## 开始准备

Yii提供了强大的配置机制和很多现成的类库。在Yii中使用RBAC是很简单的，完全不需要再写RBAC代码。所以准备工作就是，打开编辑器，跟我来。

## 设置参数、建立数据库

在配置数组中，增加以下内容：

```php
<?php
'components' => array(
	……
	'authManager'=>array(
		'class'=>'CDbAuthManager',//认证类名称
		'defaultRoles'=>array('guest'),//默认角色
		'itemTable' => 'pre_auth_item',//认证项表名称
		'itemChildTable' => 'pre_auth_item_child',//认证项父子关系
		'assignmentTable' => 'pre_auth_assignment',//认证项赋权关系
	),
	……
)
?>
```

那这三个数据表怎么建立呢？很简单，去看framework/web/auth/schema.sql。注意要和你的自定义的表名称对应起来。比如SQL文件中的AuthItem你要修改为pre_auth_item。然后在数据库中运行这个SQL文件中的语句。

## 了解概念

你可能要问，剩下的代码呢？我告诉你，没有啦。RBAC系统就这样建立起来了。但是为了使用它，你需要了解它的运行机制。我会尽量讲的啰嗦一点……（官方的RBAC文档在这里，但是我曾经看了4-5遍才明白。）

### 三个概念

你需要了解的是，授权项目可分为operations（行动）,tasks（任务）和 roles（角色）。

一个用户拥有一个或者多个角色，比如，我们这里有三个角色：银行行长、银行职员、顾客。我们假设：

* 张行长 有角色：银行行长、银行职员、顾客（人家自己可以存钱嘛）。
* 王职员 有角色：银行职员、顾客。
* 小李 有角色：顾客。

那么，相应的，只要顾客可以做的事情，小李就可以做，王职员和张行长也可以。银行职员可以做的事情，王职员和张行长都可以做，小李就不可以了。

比如，一个“顾客”可以存钱，那么拥有“顾客”角色的张行长、王职员、小李都可以存钱。“银行职员”可以打印顾客的交易记录，那么有“银行职员”角 色的张行长和王职员都可以，而小李不行，必须找一个有“银行职员”角色的人才可以打印详细的交易记录。一个“银行行长”才可以进入银行钱库提钱，那么只有 张行长可以，因为它才有“银行行长”的角色。

这就是基于角色的认证体系，简称RBAC。

### 角色的继承

角色是可以继承的，比如我们规定如下：

* 凡是“银行行长”都是“银行职员”，也就是说，只要银行职员可以做的事情，银行行长都可以做。
* 凡是“银行职员”都是顾客，同上，顾客可以做的事情银行职员也可以做。

那么角色关系就变成了：

* 张行长 有角色：银行行长。
* 王职员 有角色：银行职员。
* 小李 有角色：顾客。

这样更简单了，这就是角色的继承。

### 任务的继承

一个任务（task）是可以包含另外一个任务的，我们举个例子，比如“进入银行”。

我们设定“顾客”这个角色有“进入银行”的权限。也就是说，“顾客”可以执行“进入银行”的任务。接下来，我们假设“进入柜台”是进入银行的父权 限，也就是说，“进入柜台”包含“进入银行”。只要能“进入柜台”的人都可以“进入银行”。我们把“进入柜台”这个任务权限给“银行职员”。

那么从角色上来说，王职员可以进入银行，因为王职员的角色是“银行职员”，而“银行职员”包含了“顾客”的角色。那么“顾客”可以进行的“任务”对于“银行职员”来说也是可以进行的。而“顾客”可以“进入银行”，那么王职员也可以“进入银行”。这是角色的继承带来的。

我们再假设有个赵领导，是上级领导，可以进入柜台进行视察。那么，我们的任务关系是：

* 赵领导 有任务：进入柜台。

那么，赵领导就可以“进入银行”。因为“进入银行”是被“进入柜台”包含的任务。只要可以执行“进入柜台”的人都可以执行“进入银行”。这就是任务的继承。

### 关于行动

行动是不可划分的一级。也就是说。而一个行动是不能包含其他行动的。假设我们有个行动叫“从银行仓库中提钱”。我们把这个行动作包含“进入柜台”。那么只要可以执行“从银行仓库中提钱”的角色都可以执行“进入柜台”这个任务。

### 三者关系

* 一个角色可以包含另外一个或者几个角色。
* 一个任务可以包含另外一个或者几个行动。
* 一个行动只能被角色或者任务包含，行动是不可以包含其他，也不可再分。

这样，就形成了一个权限管理体系。关于“任务”和“行动”，你不必思考其字面上的意义。这两者就是形成两层权限。

## 进行赋权

我们建立了RBAC权限管理，就需要进行对权限的WEB管理。这些就需要你自己写代码了。

根据不同种类的项目调用下列方法之一定义授权项目：

* CAuthManager::createRole
* CAuthManager::createTask
* CAuthManager::createOperation

一旦我们拥有一套授权项目，我们可以调用以下方法建立授权项目关系：

* CAuthManager::addItemChild
* CAuthManager::removeItemChild
* CAuthItem::addChild
* CAuthItem::removeChild

最后，我们调用下列方法来分配角色项目给各个用户：

* CAuthManager::assign
* CAuthManager::revoke

下面我们将展示一个例子是关于用所提供的API建立一个授权等级：

```php
<?php
$auth=Yii::app()->authManager;
$auth->createOperation('createPost','create a post');
$auth->createOperation('readPost','read a post');
$auth->createOperation('updatePost','update a post');
$auth->createOperation('deletePost','delete a post');
$bizRule='return Yii::app()->user->id==$params["post"]->authID;';
$task=$auth->createTask('updateOwnPost','update a post by author himself',$bizRule);
$task->addChild('updatePost');
$role=$auth->createRole('reader');
$role->addChild('readPost');
$role=$auth->createRole('author');
$role->addChild('reader');
$role->addChild('createPost');
$role->addChild('updateOwnPost');
$role=$auth->createRole('editor');
$role->addChild('reader');
$role->addChild('updatePost');
$role=$auth->createRole('admin');
$role->addChild('editor');
$role->addChild('author');
$role->addChild('deletePost');
$auth->assign('reader','readerA');
$auth->assign('author','authorB');
$auth->assign('editor','editorC');
$auth->assign('admin','adminD');
?>
```

也就是说，你需要自己写一个管理界面，来列出你的角色、任务、行动，然后可以在这个界面上进行管理。比如增加、删除、修改。

## 权限检查

假设你在你的管理界面进行了赋权，那么可以在程序里面进行权限检查：

```php
<?php
if( Yii::app()->user->checkAccess('createPost') ) {
	// 这里可以显示表单等操作
} else {
	// 检查没有通过的可以跳转或者显示警告
}
?>
```

上面的代码就检查了用户是否可以执行“createPost”，这createPost可能是一个任务，也可以是一个行动。

## 其他的

对于很多说Yii权限体系RBAC不好用的人其实都没有看懂文档。综合我的体验，我感觉Yii框架的RBAC是我用过的框架里面最好用的。而且是需要自己写代码最少的。

Yii的RBAC有更加高级的用法，比如“业务规则”，“默认角色”。你可以去参考官方文档。

在具体的权限判断的时候，使用了user组件的checkAccess方法。但是在使用的时候发现，虽然这个方法是很方便的，但是总不能在每个Action里面都写上权限判断吧，那么每个Action中都会出现以下的代码：

```php
<?php
if(Yii::app()->user->checkAccess('admin')) {
	//验证通过，进行操作
} else {
	//验证不通过，进行登录或者抛出错误页面
}
?>
```

重复这样的步骤很是令人崩溃。其实，在日常使用的时候经常是对于Action级别进行权限判断。在Yii中早已准备好了现成的代码：

```php
<?php
class XXXController extends CController
{
	public function filters()
	{
		return array('accessControl');
	}

	public function accessRules()
	{
		return array(
			array('allow', 'roles'=>array('admin', 'editor'))
		);
	}
}
?>
```

accessControl是一个过滤器，会在controller执行的时候进行权限判断。权限规则写在accessRules里面。上面的代码就对所有Action只允许有admin和editor角色的用户访问。

进一步书写accessRules，你可以精确控制所有的权限。从IP到用户名再到角色，不同的Action进行不同的权限判断，都可以在规则里面写出来。

关于这个规则的具体写法，官方的指南里面有写，具体位置在：Special Topics => Authentication and Authorization。中文版在：http://yiiframework.com/doc/guide/zh_cn/topics.auth

默认的Yii的授权filter:accessControl。它是根据accessRules按照用户的身份(users)来验证并授权的，默认 的有*(任何用户),@(通过登录验证的用户),admin(管理员)。通过一系列这些accessRules，指定了这三种身份用户的各自可调用的 action的权限。

而RBAC和默认的按照users的原理一样。通过指定”roles”来限定对应角色用户的可用action的权限。用”roles”替 换”users”即可，因为Yii的accessControl是支持roles的。通过这种方式，在Contoller内部，通过指定 accessRules就可以控制权限。在其他地方，比如控制一些view的显示的时候，可以用 Yii::app()->user->checkAccess(role)来进行权限判断。

RBAC本身也有特别之处。看官方的一段原话：

在Yii的RBAC的一个基本概念是authorization item（授权项目）。一个授权项目是一个做某事的许可（如创造新的博客发布，管理用户）。根据其粒度和targeted audience， 授权项目可分为operations（行动）,tasks（任务）和 roles（角色）。角色包括任务，任务包括行动，行动是许可是个原子。 例如，我们就可以有一个administrator角色，包括post management和user management任务。user management 任务可能包括create user，update user和delete user行动。为了更灵活，Yii也可以允许角色包括其他角色和动作，任务包括其他任务，行动包括其他行动。

也就是说在Yii::app()->user->checkAccess(role)的时候，role可以是operations,tasks和roles。

可能比较麻烦的就是初始化RBAC。换句话说，就是怎样才能告诉系统，我需要设定那些角色和操作等等。

这里我使用了一个extension叫SRBAC。可以按照文档很轻松地将这个扩展加入到web应用中。

sbrac很不错的功能是，根据你现有的controllers能够很智能地生成tasks以及operations。同时通过图形化的界面，可以 方便地设定roles，tasks和operations的关系。这些roles,tasks.operations和他们之间的关系，都放在数据库里。

对应的数据库配置在config/main.php中：

```php
<?php
'components'=>array(
	……
	'authManager'=>array(
		'class'=>'CDbAuthManager',
		'connectionID'=>'db',
		'itemTable'=>'rbac_items',
		'assignmentTable'=>'rbac_assignments',
		'itemChildTable'=>'rbac_itemchildren',
	),
	……
),
'authManager'=>array(
	'class'=>'CDbAuthManager',
	'connectionID'=>'db',
	//
	'defaultRoles'=>array('Trainer', 'authenticated', 'Guest'),
	'itemTable'=>'rbac_items',
	'assignmentTable'=>'rbac_assignments',
	'itemChildTable'=>'rbac_itemchildren',
),
?>
```

表的内容很简单，一看就明白了就不多说了。srbac的使用其实也很简单，感觉也不必多说。

通过srbac，可以对现有的用户、role、task、operation进行管理。

在你自己的controller中，你也可以通过访问数据库来修改权限。

这里可以使用一个现成的model，就是srbac中的models/Assignments.php来改变用户和role的关系。比如我在新建一个用户的时候，可以：

```php
<?php
$auth = new Assignments;
$auth->itemname = $model->type;
$auth->userid = $model->id;
$auth->save();
?>
```
