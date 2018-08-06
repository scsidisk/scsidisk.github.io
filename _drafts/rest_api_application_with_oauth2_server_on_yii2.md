REST API application with OAuth2 server on Yii2的完整实现流程
=============================================================

by  [shuijingwan](http://www.shuijingwanwq.com/author/shuijingwan/ "由shuijingwan发布")
 · 2015/08/26









1、基于https://github.com/Filsh/yii2-oauth2-server实现；

运行：php composer.phar require –prefer-dist filsh/yii2-oauth2-server
“\*”



[![安装yii2-oauth2-server](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/13.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/13.png)
安装yii2-oauth2-server



2、在应用程序中配置：

E:\\wwwroot\\api.hmwis.com\\passport\\config\\main.php

‘modules’ =&gt; \[\
 ‘oauth2’ =&gt; \[\
 ‘class’ =&gt; ‘filsh\\yii2\\oauth2server\\Module’,\
 ‘tokenParamName’ =&gt; ‘accessToken’,\
 ‘tokenAccessLifetime’ =&gt; 3600 \* 24,\
 ‘storageMap’ =&gt; \[\
 ‘user\_credentials’ =&gt; ‘common\\models\\User’,\
 \],\
 ‘grantTypes’ =&gt; \[\
 ‘user\_credentials’ =&gt; \[\
 ‘class’ =&gt; ‘OAuth2\\GrantType\\UserCredentials’,\
 \],\
 ‘refresh\_token’ =&gt; \[\
 ‘class’ =&gt; ‘OAuth2\\GrantType\\RefreshToken’,\
 ‘always\_issue\_new\_refresh\_token’ =&gt; true\
 \]\
 \]\
 \],\
 ‘v1’ =&gt; \[\
 ‘class’ =&gt; ‘passport\\modules\\v1\\Module’,\
 \],\
 \],



[![在应用程序中配置oauth2](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/23.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/23.png)
在应用程序中配置oauth2



3、编辑用户模型类User.php：

E:\\wwwroot\\api.hmwis.com\\common\\models\\User.php

实现接口\\OAuth2\\Storage\\UserCredentialsInterface\
 class User extends ActiveRecord implements IdentityInterface,
\\OAuth2\\Storage\\UserCredentialsInterface



[![实现接口\\OAuth2\\Storage\\UserCredentialsInterface](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/33.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/33.png)
实现接口\\OAuth2\\Storage\\UserCredentialsInterface



3.1、基于邮箱、手机查找对应用户：



[![基于邮箱、手机查找对应用户](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/3.11.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/3.11.png)
基于邮箱、手机查找对应用户



3.2、实现接口类中的两个方法：



[![实现接口类中的两个方法](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/3.2.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/3.2.png)
实现接口类中的两个方法



4、运行数据迁移：

运行：yii migrate
–migrationPath=@vendor/filsh/yii2-oauth2-server/migrations



[![PHP Strict Warning 'yii\\base\\ErrorException' with message
'Declaration of m14050 1\_075311\_add\_oauth2\_server::primaryKey()
should be compatible with yii\\db\\Migrat ion::primaryKey(\$length =
NULL)'](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/42.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/42.png)
PHP Strict Warning ‘yii\\base\\ErrorException’ with message ‘Declaration
of m14050\
1\_075311\_add\_oauth2\_server::primaryKey() should be compatible with
yii\\db\\Migrat\
ion::primaryKey(\$length = NULL)’



5、编辑m140501\_075311\_add\_oauth2\_server.php：

public function primaryKey(\$columns = null)



[![编辑m140501\_075311\_add\_oauth2\_server.php](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/51.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/51.png)
编辑m140501\_075311\_add\_oauth2\_server.php



6、再次运行：yii migrate
–migrationPath=@vendor/filsh/yii2-oauth2-server/migrations



[![再次运行：yii migrate
--migrationPath=@vendor/filsh/yii2-oauth2-server/migrations](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/6.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/6.png)
再次运行：yii migrate
–migrationPath=@vendor/filsh/yii2-oauth2-server/migrations



6.1、查看数据库中已经存在相应数据表：



[![查看数据库中已经存在相应数据表](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/6.1.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/6.1.png)
查看数据库中已经存在相应数据表



7、添加URL规则到urlManager：

E:\\wwwroot\\api.hmwis.com\\passport\\config\\main-local.php

‘POST oauth2/&lt;action:\\w+&gt;’ =&gt; ‘oauth2/rest/&lt;action&gt;’,



[![添加URL规则到urlManager](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/7.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/7.png)
添加URL规则到urlManager



8、要使用该扩展，只需添加行为到您的基本控制器：



[![要使用该扩展，只需添加行为到您的基本控制器](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/8.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/8.png)
要使用该扩展，只需添加行为到您的基本控制器



9、http://passport.api.hmwis.com/oauth2/token



[!["SQLSTATE\[42S02\]: Base table or view not found: 1146 Table
'api\_hmwis\_com.oauth\_clients' doesn't
exist"](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/9.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/9.png)
“SQLSTATE\[42S02\]: Base table or view not found: 1146 Table
‘api\_hmwis\_com.oauth\_clients’ doesn’t exist”



10、E:\\wwwroot\\api.hmwis.com\\vendor\\filsh\\yii2-oauth2-server\\storage\\Pdo.php
\$this-&gt;config = array\_merge(array(\
 ‘client\_table’ =&gt; \\Yii::\$app-&gt;db-&gt;tablePrefix .
‘oauth\_clients’,\
 ‘access\_token\_table’ =&gt; \\Yii::\$app-&gt;db-&gt;tablePrefix .
‘oauth\_access\_tokens’,\
 ‘refresh\_token\_table’ =&gt; \\Yii::\$app-&gt;db-&gt;tablePrefix .
‘oauth\_refresh\_tokens’,\
 ‘code\_table’ =&gt; \\Yii::\$app-&gt;db-&gt;tablePrefix .
‘oauth\_authorization\_codes’,\
 ‘user\_table’ =&gt; \\Yii::\$app-&gt;db-&gt;tablePrefix .
‘oauth\_users’,\
 ‘jwt\_table’  =&gt; \\Yii::\$app-&gt;db-&gt;tablePrefix .
‘oauth\_jwt’,\
 ‘jti\_table’  =&gt; \\Yii::\$app-&gt;db-&gt;tablePrefix .
‘oauth\_jti’,\
 ‘scope\_table’  =&gt; \\Yii::\$app-&gt;db-&gt;tablePrefix .
‘oauth\_scopes’,\
 ‘public\_key\_table’  =&gt; \\Yii::\$app-&gt;db-&gt;tablePrefix .
‘oauth\_public\_keys’,\
 ), \$config);



[![设置数据表前缀](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/10.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/10.png)
设置数据表前缀



11、http://passport.api.hmwis.com/oauth2/token

请求成功：





[![请求访问令牌成功](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/111.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/111.png)
请求访问令牌成功



12、E:\\wwwroot\\api.hmwis.com\\passport\\controllers\\UserController.php

public function checkAccess(\$action, \$model = null, \$params = \[\])\
 \
 }



[![检查访问方法，判断访问令牌所有者是否为请求用户ID](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/121.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/121.png)
检查访问方法，判断访问令牌所有者是否为请求用户ID



12.1、如果访问令牌所有者与当前用户不是同一人，则提示错误：



[![如果访问令牌所有者与当前用户不是同一人，则提示错误](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/12.2.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/12.2.png)
如果访问令牌所有者与当前用户不是同一人，则提示错误



13、编辑oauth\_clients表：



[![编辑oauth\_clients表，设置客户端授权](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/13.1.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/13.1.png)
编辑oauth\_clients表，设置客户端授权



14、设置访问令牌与刷新令牌的有效期分别为7天与30天

E:\\wwwroot\\api.hmwis.com\\vendor\\filsh\\yii2-oauth2-server\\Module.php



[![设置访问令牌与刷新令牌的有效期分别为7天与30天](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/14.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/14.png)
设置访问令牌与刷新令牌的有效期分别为7天与30天





[![设置访问令牌与刷新令牌的有效期分别为7天与30天](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/14.1.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/14.1.png)
设置访问令牌与刷新令牌的有效期分别为7天与30天



15、通过密码凭据获取访问令牌

http://passport.api.hmwis.com/oauth2/token

如果grant\_type = authorization\_code\
 请求失败：





[![如果grant\_type = authorization\_code
请求失败](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/15.0.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/15.0.png)
如果grant\_type = authorization\_code\
请求失败



15.1、获取访问令牌成功，且在数据库中进行确认：



[![获取访问令牌成功](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/15.01.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/15.01.png)
获取访问令牌成功





[![确认访问令牌成功](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/15.1.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/15.1.png)
确认访问令牌成功





[![确认刷新令牌成功](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/15.2.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/15.2.png)
确认刷新令牌成功



16、通过刷新令牌获取访问令牌

http://passport.api.hmwis.com/oauth2/token



[![通过刷新令牌获取访问令牌](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/16.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/16.png)
通过刷新令牌获取访问令牌



17、修改用户个人信息

http://passport.api.hmwis.com/v1/users/4

测试访问令牌：



[![测试访问令牌，错误的](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/17.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/17.png)
测试访问令牌，错误的





[![测试访问令牌，正确的](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/17.1.png)](http://www.shuijingwanwq.com/wp-content/uploads/2015/08/17.1.png)
测试访问令牌，正确的



 

 

























