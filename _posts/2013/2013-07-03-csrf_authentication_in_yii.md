---
layout: post
title: Yii 中的 CSRF 验证
date: 2013-07-03
author: scsidisk
category: PHP
tags: PHP, Yii
---

  
CSRF(Cross-site request forgery：跨站请求伪造)，也被称成为"one click attack"或者session riding，通常缩写为CSRF或者XSRF，是一种对网站的恶意利用。尽管听起来像跨站脚本（XSS），但它与XSS非常不同，并且攻击方式几乎相左。XSS利用站点内的信任用户，而CSRF则通过伪装来自受信任用户的请求来利用受信任的网站。与XSS攻击相比，CSRF攻击往往不大流行（因此对其进行防范的资源也相当稀少）和难以防范，所以被认为比XSS更具危险性。
  
## 示例和特性
  
CSRF攻击通过在授权用户访问的页面中包含链接或者脚本的方式工作。例如：一个网站用户Bob可能正在浏览聊天论坛，而同时另一个用户Alice 也在此论坛中，并且后刚刚发布了一个具有Bob银行链接的图片消息。设想一下，Alice编写了一个在Bob的银行站点上进行取款的form提交的链接，并将此链接作为图片tag。如果Bob的银行在cookie中保存他的授权信息，并且此cookie没有过期，那么当Bob的浏览器尝试装载图片时将提交这个取款form和他的cookie，这样在没经Bob同意的情况下便授权了这次事务。
  
CSRF是一种依赖web浏览器的、被混淆过的代理人攻击（deputy attack）。在上面银行示例中的代理人是Bob的web浏览器，它被混淆后误将Bob的授权直接交给了Alice使用。
  
下面是CSRF的常见特性：

- 依靠用户标识危害网站
- 利用网站对用户标识的信任
- 欺骗用户的浏览器发送HTTP请求给目标站点
- 风险在于那些通过基于受信任的输入form和对特定行为无需授权的已认证的用户来执行某些行为的web应用。已经通过被保存在用户浏览器中的cookie进行认证的用户将在完全无知的情况下发送HTTP请求到那个信任他的站点，进而进行用户不愿做的行为。
- 使用图片的CSRF攻击常常出现在网络论坛中，因为那里允许用户发布图片而不能使用JavaScript。
  
## 防范措施
  
对于web站点，将持久化的授权方法（例如cookie或者HTTP授权）切换为瞬时的授权方法（在每个form中提供隐藏field），这将帮助网站防止这些攻击。一种类似的方式是在form中包含秘密信息、用户指定的代号作为cookie之外的验证。
  
另一个可选的方法是"双提交"cookie。此方法只工作于Ajax请求，但它能够作为无需改变大量form的全局修正方法。如果某个授权的 cookie在form post之前正被JavaScript代码读取，那么限制跨域规则将被应用。如果服务器需要在Post请求体或者URL中包含授权cookie的请求，那么这个请求必须来自于受信任的域，因为其它域是不能从信任域读取cookie的。
  
与通常的信任想法相反，使用Post代替Get方法并不能提供卓有成效的保护。因为JavaScript能使用伪造的POST请求。尽管如此，那些导致对安全产生"副作用"的请求应该总使用Post方式发送。Post方式不会在web服务器和代理服务器日志中留下数据尾巴，然而Get方式却会留下数据尾巴。
  
尽管CSRF是web应用的基本问题，而不是用户的问题，但用户能够在缺乏安全设计的网站上保护他们的帐户：通过在浏览其它站点前登出站点或者在浏览器会话结束后清理浏览器的cookie。
  
## 影响CSRF的因素
  
- CSRF攻击依赖下面的假定：
- 攻击者了解受害者所在的站点
- 攻击者的目标站点具有持久化授权cookie或者受害者具有当前会话cookie
- 目标站点没有对用户在网站行为的第二授权
  
  
In Yii, how to start the CSRF authorization? It is very easy to do that.

Just add this to main.php
  
```
'components'=>array( 
	'request'=>array( 
		'enableCsrfValidation'=>true, 
	), 
), 
```
  
And then, do something else to send a request to the server, you have to provide the YII_CSRF_TOKEN ( the browser will do for us when click a link), otherwise, you will get this message
  
```
The CSRF token could not be verified. 
```
  
when you post a form, if you do not use CActiveForm or its children, you have to provide a hidden field to store the YII_CSRF_TOKEN.
  
```
<input type="hidden" name="YII_CSRF_TOKEN" value="<?php echo Yii::app()->request->csrfToken; ?>" />
```
  
If you use CActiveForm or its children, you just use the same code no matter you set enableCsrfValidation to true or false.
  
```
<?php $form=$this->beginWidget('CActiveForm'); ?>
```
  
Yii will know how to do it!

Have fun with Yii! :)
  
以上内容转载自：http://cnblogs.com/davidhhuan/archive/2011/01/19/1939253.html
  
今天在项目中开启了 enableCsrfValidation
  
结果发现选择一级分类后，无法提取二级分类的内容。通过抓包，得到：csrf token 无法被验证。解决办法：要在提交数据中附上 `YII_CSRF_TOKEN`
  
```
<tr>
	<td width="10%"><?php echo $form->labelEx($model, 'sid')?></td>
	<td width="90%"><div class="mm_div_left"><?php echo CHtml::activeDropDownList($model, 
		'fid', 
		Costcategory::getCategory(), 
		array( 
			'empty'=>'请选择', 
			'ajax'=>array( 
				'type'=>'POST', 
				'url'=>CController::createUrl('cost/dynamiccities'), 
				'update'=>'#Cost_sid', 
				'data'=>array('fid'=>'js:this.value','YII_CSRF_TOKEN'=>Yii::app()->request->csrfToken), 
			) 
		)); 
	echo CHtml::activeDropDownList($model, 'sid', 
		Costcategory::getCategory($model->fid), 
		array( 
			'empty'=>'请选择', 
		) 
	); ?>
	</div>
	<div class="mm_div_right"><?php echo $form->error($model, 'sid');?></div>
	</td>
</tr>
``` 
