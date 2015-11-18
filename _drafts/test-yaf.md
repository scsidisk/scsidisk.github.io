Yaf中PHPUnit单元测试最佳实践
==========================

2015-9-10 12:06

http://blog.phpdr.net/yaf%e4%b8%adphpunit%e5%8d%95%e5%85%83%e6%b5%8b%e8%af%95%e6%9c%80%e4%bd%b3%e5%ae%9e%e8%b7%b5.html

### public同级目录建立tests目录

tests/phpunit.xml

```xml
<phpunit bootstrap="./index.php">
    <testsuites>
        <testsuite name="controllers">
            <file>./controllers/TestIndex.php</file>
        </testsuite>
    </testsuites>
</phpunit>
```

tests/index.php

```php
<?php
define ( 'APP_PATH', dirname ( __DIR__ ) . '/app', true );
(new Yaf_Application ( APP_PATH . '/conf/app.ini' ))->bootstrap ();
```

tests/controllers/TestIndex.php

```php
<?php
class TestIndex extends PHPUnit_Framework_TestCase {
    public function testIndex() {
        $request=new Yaf_Request_Simple('CLI','','Index','test');
        $res=Yaf_Application::app()->getDispatcher()->returnResponse(true)->dispatch($request);
        $valid='test string';
        $this->assertEquals ( $valid, $res->getBody());
    }
}
```

APP\_PATH.'/controllers/Index.php'

```php
<?php
class IndexController extends Yaf_Controller_Abstract {
    function testAction(){
        $this->getResponse()->setBody('test string');
    }
}
```

### 运行

```
[root@ares tests]# phpunit
PHPUnit 4.8.6 by Sebastian Bergmann and contributors.

.

Time: 105 ms, Memory: 4.00Mb

OK (1 test, 1 assertion)
```

### 参考

http://www.crackedzone.com/phpunit-yaf-controller.html




