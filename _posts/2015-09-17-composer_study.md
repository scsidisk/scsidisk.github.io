2015年PHP最常用的类库
===================

composer分析

2015-09-17

转自: http://oabeta.com/popular-library-in-php-develop-2015/

注：统计数据来自packagist.org的70000余个项目的composer信息

做测试时建议同时使用以获得更好效果的类库

mockery+codesniffer+coveralls+phpmd+vfsstream+doctrine+php-test-reporter+faker+codeception

    phpunit/phpunit                 占比例如此之高，其他类库无出其右，
                                    也可看出php的单元测试应用已然成为主流
    mockery/mockery                 mock工具
    squizlabs/php\_codesniffer      代码自动审查功能不可或缺
    satooshi/php-coveralls          覆盖率工具
    phpspec/phpspec                 是 PHP 的 SpecBDD 框架，是通过规范异常驱动的 PHP 测试框架。
    phpmd/phpmd                     代码检查工具
    fabpot/php-cs-fixer             自动规范化你的 PHP 代码，
                                    可以依据fig规范做自动检查，功能类同php\_codesniffer
    nette/tester                    单元测试工具
    symfony/yaml                    YAML是一种所有编程语言可用的友好的数据序列化标准，
                                    一种轻量化的数据描述语言，在各种应用场景下都有适用范围
    mikey179/vfsstream              虚拟文件系统，mock工具
    doctrine/orm                    是基于数据库抽像层上的ORM,它可以通过PHP对象轻松访问所有的数据库，
                                    做数据库操作实在太强大了，所以在开发测试过程中也被大量采用
    codeclimate/php-test-reporter   生成php测试结果报告工具
    sebastian/phpcpd                提供识别代码中重复代码的能力，分析检测常用工具
    behat/behat                     Behat 是个行为驱动的开发（BDD）框架，可以测试业务期望，
                                    允许用户编写便于人们阅读的故事驱动代码，描述该应用应该怎样工作。
                                    任何人都能快速简单的掌握它的使用方法。
    symfony/browser-kit             模拟浏览器行为的工具
    fzaninotto/faker                批量创建模拟数据
    orchestra/testbench             orchestra提供了一整套laravel框架的扩展，
                                    testbench是一套很容易整合在laravel中的基于phpunit的单元测试框架
    codeception/codeception         Phpspec 用过 Rails 的 rspec 的朋友应该会习惯这种写测试的方法;
                                    Behat 可读性最高的测试, 非程序员使用;
                                    Codeception 全堆栈的 PHP 测试框架, 提供测试的方法多样, 灵活.
    phpdocumentor/phpdocumentor     这个大部分人都知道在php代码里写注释就能通过他自动生成api文档了
    twig/twig                       模版系统


其他类库不做介绍，有兴趣的自行google

这里笔者多唠叨两句，PHP开发人员这些年的平均素质在稳步升高，相信很少再有人还认为php只能做小规模项目开发，实际上各种巨型、明星互联网产品都在大量使用php开发，而且由于优秀的开源库涌现，在PHP开发流程中，单元测试、集成测试等自动化测试也越来越容易，大型团队开发更加灵活，项目代码质量更有保障。

谁还在给团队新成员培训自己制定的一堆代码规范准则，psr-0 \~ psr-4大家一看就明白了

谁还在做大量的黑盒和回归测试，单元测试已经大幅减少了bug

