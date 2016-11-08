---
layout: post
title: PHPUnit 断言和标注
date: 2016-11-08
author: scsidisk
category: PHP
tags: PHPUnit Test
---

## 1. 断言

在PHPUnit中，断言(Assertions)是其提供的一系列对程序执行结果测试的方法。

> - 绝大多数断言都有相反的 Not  断言对应， 接受相同参数
> - 都可以在最后传入 $message 参数，自定义错误信息
>
> ### 说明
> - expected 要求
> - actual 实际
> - condition 条件
> - variable 变量

### 逻辑判断

- `assertEmpty($actual)` -- 是否为空
- `assertFalse($condition)` -- 是否为false
- `assertTrue($condition)` -- 为真
- `assertNan($variable)` -- 是否为 NAN
- `assertNull($variable)` -- 是否为 NULL

### 比较

- `assertEquals($expected, $actual)` -- $expected 等于 $actual
- `assertGreaterThan($expected, $actual)` -- $expected 大于 $actual
- `assertGreaterThanOrEqual($expected, $actual)` -- $expected大于等于$actual
- `assertLessThan($expected, $actual)` -- $expected 小于 $actual
- `assertLessThanOrEqual($expected, $actual)` -- $expected 小于等于 $actual
- `assertSame($expected, $actual)` -- 是否完全相同
- `assertInstanceOf($expected, $actual)` -- $actual是$expected的实例

### 验证

- `assertRegExp($pattern, $string)` -- 匹配正则表达式
- `assertStringMatchesFormat($format, $string)` -- 匹配格式
    - 格式定义字符串中可以使用如下占位符：
    - %e：表示目录分隔符，例如在 Linux 系统中是 /。
    - %s：一个或多个除了换行符以外的任意字符（非空白字符或者空白字符）。
    - %S：零个或多个除了换行符以外的任意字符（非空白字符或者空白字符）。
    - %a：一个或多个包括换行符在内的任意字符（非空白字符或者空白字符）。
    - %A：零个或多个包括换行符在内的任意字符（非空白字符或者空白字符）。
    - %w：零个或多个空白字符。
    - %i：带符号整数值，例如 +3142、-3142。
    - %d：无符号整数值，例如 123456。
    - %x：一个或多个十六进制字符。所谓十六进制字符，指的是在以下范围内的字符：0-9、a-f、A-F。
    - %f：浮点数，例如 3.142、-3.142、3.142E-10、3.142e+10。
    - %c：单个任意字符。
- `assertStringEndsWith($suffix, $string)` -- $string 以 $suffix 结尾
- `assertStringStartsWith($suffix, $string)` -- $string 不以 $prefix 开头

### 文件相关

- `assertFileEquals($filename, $filename)` -- 文件内容是否相同
- `assertFileExists($filename)` -- 文件是否存在
- `assertFileIsWritable($filename)` -- $filename 所指定的文件不是个文件且可写
- `assertFileIsReadable($filename)` -- $filename 所指定的文件不是个文件且可读
- `assertDirectoryExists($directory)` -- $directory 所指定的目录存在
- `assertDirectoryIsReadable($directory)` -- $directory 所指定的目录是个目录且可读
- `assertDirectoryIsWritable($directory)` -- $directory 所指定的目录是个目录且可写
- `assertIsReadable($filename)` -- $filename 所指定的文件或目录可读
- `assertIsWritable($filename)` -- $filename 所指定的文件或目录可写
- `assertStringEqualsFile($expectedFile, $actualString)` -- $expectedFile 文件内容为 $actualString
- `assertStringMatchesFormatFile($formatFile, $string)` -- $string 匹配于 $formatFile的内容所定义的格式`

### XML相关

- `assertEqualXMLStructure($expectedElement, $actualElement[, $checkAttributes = false])` -- $actualElement 与 $expectedElement 的 XML 结构相同
- `assertXmlFileEqualsXmlFile($expectedFile, $actualFile)` -- $actualFile 与 $expectedFile 对应的 XML文档相同`
- assertXmlStringEqualsXmlFile($expectedFile, $actualXml) --
$actualXml 与 $expectedFile 对应的 XML 文档相同`
- `assertXmlStringEqualsXmlString($expectedXml, $actualXml)` -- $actualXml 与 $expectedXml 对应的 XML文档相同`

### json 相关

- `assertJsonFileEqualsJsonFile($expectedFile, $actualFile)` -- $actualFile 与 $expectedFile 匹配
- `assertJsonStringEqualsJsonFile($expectedFile, $actualJson)` -- $actualJson 与 $expectedFile 匹配
- `assertJsonStringEqualsJsonString($expectedJson, $actualJson)` -- $actualJson 与 $expectedJson 匹配

### 其他

- `assertArrayHasKey($key, $array)` -- $array 包含 $key
- `assertClassHasAttribute($attributeName, $className)` -- $className::attributeName 存在
- `assertArraySubset($subset, $array[, $strict = ''])` -- $array 包含 $subset
- `assertClassHasStaticAttribute($attributeName, $className)` -- $className::attributeName 存在
- ``assertContains($needle, Iterator|$haystack)` -- $needle 是 $haystack的元素
- `assertContainsOnly($type, Iterator|$haystack)` -- $haystack 仅包含类型为 $type 的变量
- `assertContainsOnlyInstancesOf($classname, Traversable|$haystack)` -- $haystack 仅包含类 $classname的实例`
- `assertCount($expectedCount, $haystack)` -- $haystack 中的元素数量是 $expectedCount
- `assertInfinite($variable)` -- $actual 是 INF
- `assertInternalType($type, $actual)` -- $expected 的类型是 $actual
- `assertObjectHasAttribute($attributeName, $object)` -- $object->attributeName 存在
- `assertThat($value, $constraint)` -- $value 符合约束条件 $constraint

## 2. 标注

- @author 标注是 @group 标注（参见 “@group”一节）的别名，允许基于作者对测试进行过滤。
- @after 标注用于指明此方法应当在测试用例类中的每个测试方法运行完成之后调用。
- @afterClass 标注用于指明此静态方法应该于测试类中的所有测试方法都运行完成之后调用，用于清理共享基境。
- @backupGlobals 全局变量的备份与还原操作可以对某个测试用例类中的所有测试彻底禁用，也可以用在测试方法这一级别。这样可以对备份与还原操作进行更细粒度的配置
- @backupStaticAttributes 标注，那么将在每个测试之前备份所有已声明的类的静态属性的值，并在测试完成之后全部恢复。它可以用在测试用例类或测试方法级别
- @before 标注用于指明此方法应当在测试用例类中的每个测试方法开始运行之前调用。
- @beforeClass 标注用于指明此静态方法应该于测试类中的所有测试方法都运行完成之后调用，用于建立共享基境。
- @codeCoverageIgnore, @codeCoverageIgnoreStart and @codeCoverageIgnoreEnd 标注用于从覆盖率分析中排除掉某些代码行。
- @covers 标注来指明测试方法想要对哪些方法进行测试：
- @coversDefaultClass 标注用于指定一个默认的命名空间或类名，这样就不用在每个 @covers 标注中重复长名称
- @coversNothing 标注来指明所标注的测试用例不需要记录任何代码覆盖率信息。
- @dataProvider 所要使用的数据供给器方法用 @dataProvider 标注来指定。
- @depends 标注来表达测试方法之间的依赖关系。
- @expectedException
- @expectedExceptionCode 标注与 @expectedException 联合使用，可以对抛出异常的代码作出断言，这样可以缩小具体异常的范围。
- @expectedExceptionMessage 标注的运作方式类似于 @expectedExceptionCode ，用它可以对异常的错误讯息作出断言。
- @expectedExceptionMessageRegExp 预期讯息也可以通过 @expectedExceptionMessageRegExp 标注以正则表达式来指定。当无法用子串来完成对给定讯息的匹配时，这种方式就非常有用了。
- @group 标注来标记为属于一个或多个组
- @large 标注是 @group large 的别名。
- @medium 标注是 @group medium 的别名。中型(medium)测试不能依赖于标记为 @large 的测试。
- @preserveGlobalState 标注来禁止 PHPUnit 保持全局状态。
- @requires 标注用于在常规前提条件（例如 PHP 版本或所安装的扩展）不满足时跳过测试。
- @runTestsInSeparateProcesses 指明单个测试类内的所有测试要各自运行在独立的 PHP 进程中。
- @runInSeparateProcess 明某个测试要运行在独立的 PHP 进程中。
- @small 标注是 @group small 的别名。小型(small)测试不能依赖于标记为 @medium 或 @large 的测试。
- @test 除了用 test 作为测试方法名称的前缀外，还可以在方法的文档注释块中用 @test 标注来将其标记为测试方法。
- @testdox
- @ticket
- @uses 标注用来指明那些将会在测试中执行到但同时又不打算让其被测试所覆盖的代码。在对代码单元进行测试时所必须的值对象就是个很好的例子。
