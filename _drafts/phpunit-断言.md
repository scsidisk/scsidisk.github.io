PHPUnit 断言
============

### 注释

- expected 要求
- actual 实际
- condition 条件
- variable 变量

### 逻辑判断

- assertEmpty($actual) `是否为空`
- assertFalse($condition) `是否为false`
- assertTrue($condition) `为真`
- assertNan($variable) `是否为 NAN`
- assertNull($variable) `是否为 NULL`

### 比较

- assertEquals($expected, $actual) `是否相等`
- assertGreaterThan($expected, $actual) `$expected 大于 $actual`
- assertGreaterThanOrEqual($expected, $actual) `$expected大于等于$actual`
- assertLessThan($expected, $actual) `$expected 小于 $actual`
- assertLessThanOrEqual($expected, $actual) `$expected 小于等于 $actual`
- assertSame($expected, $actual) `是否完全相同`
- assertInstanceOf($expected, $actual) `$actual是$expected的实例`

### 验证
- assertRegExp($pattern, $string) `匹配正则表达式`
- assertStringMatchesFormat($format, $string) `匹配格式`
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
- assertStringEndsWith($suffix, $string) `$string 以 $suffix 结尾`
- assertStringStartsWith($suffix, $string) `$string 不以 $prefix 开头`

### 文件相关

- assertFileEquals() `文件内容是否相同`
- assertFileExists() `文件是否存在`
- assertStringEqualsFile($expectedFile, $actualString) `$expectedFile 文件内容为 $actualString`
- assertStringMatchesFormatFile()

### XML相关
- assertEqualXMLStructure()
- assertXmlFileEqualsXmlFile()
- assertXmlStringEqualsXmlFile()
- assertXmlStringEqualsXmlString()

### json 相关
- assertJsonFileEqualsJsonFile()
- assertJsonStringEqualsJsonFile()
- assertJsonStringEqualsJsonString()

### 其他
- assertArrayHasKey()
- assertClassHasAttribute()
- assertArraySubset()
- assertClassHasStaticAttribute()
- assertContains()
- assertContainsOnly()
- assertContainsOnlyInstancesOf()
- assertCount()
- assertInfinite()
- assertInternalType($type, $actual)
- assertObjectHasAttribute()
- assertThat()
