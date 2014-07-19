---
layout: post
title: Junit使用教程
date: 2014-07-19
author: scsidisk
category: Java
tags: Java Maven Junit
---

环境：

```
Mac OS X 1.9.2
java version "1.6.0_65"
Apache Maven 3.2.1
Junit 4.11
```

几乎所有程序员都听说过Junit的大名，我是一个菜鸟，特此总结整理了下Junit的知识点。

### 一、建立Junit测试类

1. 右击test测试包，选择New-->Oher...
2. 在窗口中找到Junit，选择Junit Test Case
3. 输入名称(Name)，命名规则一般建议采用：类名+Test。Browse...选择要测试的类，这里是StudentService。点击Next
4. 勾选要测试的方法
5. 生成后，效果如下：

```java
package test;

import junit.framework.TestCase;
import org.junit.*;

public class BookTest extends TestCase
{

    @Test
    public final void testGetId() {
        assertEquals("failure - strings not same", 5l, 5l);
    }

}
```

其实不通过Junit新建向导来建立也可以，随便建立一个新类后，只需在方法上加入@Test注解即可。

### 断言

断言是编写测试用例的核心实现方式，即期望值是多少，测试的结果是多少，以此来判断测试是否通过。

1. 断言核心方法

| 方法                                  | 说明                                               |
| ------------------------------------- | ------------------------------------------------- |
| assertArrayEquals(expecteds, actuals) | 查看两个数组是否相等。                               |
| assertEquals(expected, actual)        | 查看两个对象是否相等。类似于字符串比较使用的equals()方法 |
| assertNotEquals(first, second)        | 查看两个对象是否不相等。                              |
| assertNull(object)                    | 查看对象是否为空。                                   |
| assertNotNull(object)                 | 查看对象是否不为空。                                 |
| assertSame(expected, actual)          | 查看两个对象的引用是否相等。类似于使用“==”比较两个对象   |
| assertNotSame(unexpected, actual)     | 查看两个对象的引用是否不相等。类似于使用“!=”比较两个对象 |
| assertTrue(condition)                 | 查看运行结果是否为true。                             |
| assertFalse(condition)                | 查看运行结果是否为false。                            |
| assertThat(actual, matcher)           | 查看实际值是否满足指定的条件                          |
| fail()                                | 让测试失败                                          |
```

2. 示例

```java
package test;

import static org.hamcrest.CoreMatchers.*;
import static org.junit.Assert.*;

import java.util.Arrays;

import org.hamcrest.core.CombinableMatcher;
import org.junit.Test;

public class AssertTests {

    @Test
    public void testAssertArrayEquals() {
        byte[] expected = "trial".getBytes();
        byte[] actual = "trial".getBytes();
        org.junit.Assert.assertArrayEquals("failure - byte arrays not same", expected, actual);
    }

    @Test
    public void testAssertEquals() {
        org.junit.Assert.assertEquals("failure - strings not same", 5l, 5l);
    }

    @Test
    public void testAssertFalse() {
        org.junit.Assert.assertFalse("failure - should be false", false);
    }

    @Test
    public void testAssertNotNull() {
        org.junit.Assert.assertNotNull("should not be null", new Object());
    }

    @Test
    public void testAssertNotSame() {
        org.junit.Assert.assertNotSame("should not be same Object", new Object(), new Object());
    }

    @Test
    public void testAssertNull() {
        org.junit.Assert.assertNull("should be null", null);
    }

    @Test
    public void testAssertSame() {
        Integer aNumber = Integer.valueOf(768);
        org.junit.Assert.assertSame("should be same", aNumber, aNumber);
    }

    // JUnit Matchers assertThat
    @Test
    public void testAssertThatBothContainsString() {
        org.junit.Assert.assertThat("albumen", both(containsString("a")).and(containsString("b")));
    }

    @Test
    public void testAssertThathasItemsContainsString() {
        org.junit.Assert.assertThat(Arrays.asList("one", "two", "three"), hasItems("one", "three"));
    }

    @Test
    public void testAssertThatEveryItemContainsString() {
        org.junit.Assert.assertThat(Arrays.asList(new String[] { "fun", "ban", "net" }), everyItem(containsString("n")));
    }

    // Core Hamcrest Matchers with assertThat
    @Test
    public void testAssertThatHamcrestCoreMatchers() {
        assertThat("good", allOf(equalTo("good"), startsWith("good")));
        assertThat("good", not(allOf(equalTo("bad"), equalTo("good"))));
        assertThat("good", anyOf(equalTo("bad"), equalTo("good")));
        assertThat(7, not(CombinableMatcher.<Integer> either(equalTo(3)).or(equalTo(4))));
        assertThat(new Object(), not(sameInstance(new Object())));
    }
}
```

### 三、核心——注解

1. 说明

| 方法              | 说明                                               |
| ----------------- | ------------------------------------------------- |
| @Before           | 初始化方法                                         |
| @After            | 释放资源                                           |
| @Test             | 测试方法，在这里可以测试期望异常和超时时间             |
| @Ignore           | 忽略的测试方法                                      |
| @BeforeClass      | 针对所有测试，只执行一次，且必须为static void         |
| @AfterClass       | 针对所有测试，只执行一次，且必须为static void         |
| @RunWith          | 指定测试类使用某个运行器                             |
| @Parameters       | 指定测试类的测试数据集合                             |
| @Rule             | 允许灵活添加或重新定义测试类中的每个测试方法的行为      |
| @FixMethodOrder   | 指定测试方法的执行顺序                               |

2. 执行顺序

一个测试类单元测试的执行顺序为：

```
@BeforeClass –> @Before –> @Test –> @After –> @AfterClass
```

每一个测试方法的调用顺序为：

```
@Before –> @Test –> @After
```

3. 示例

```java
package test;

import static org.junit.Assert.*;

import org.junit.*;

public class JDemoTest {

    @BeforeClass
    public static void setUpBeforeClass() throws Exception {
        System.out.println("in BeforeClass================");
    }

    @AfterClass
    public static void tearDownAfterClass() throws Exception {
        System.out.println("in AfterClass=================");
    }

    @Before
    public void before() {
        System.out.println("in Before");
    }

    @After
    public void after() {
        System.out.println("in After");
    }

    @Test(timeout = 10000)
        public void testadd() {
        JDemo a = new JDemo();
        assertEquals(6, a.add(3, 3));
        System.out.println("in Test ----Add");
    }

    @Test
    public void testdivision() {
        JDemo a = new JDemo();
        assertEquals(3, a.division(6, 2));
        System.out.println("in Test ----Division");
    }

    @Ignore
    @Test
    public void test_ignore() {
        JDemo a = new JDemo();
        assertEquals(6, a.add(1, 5));
        System.out.println("in test_ignore");
    }

    @Test
    public void teest_fail() {
        fail();
    }
}

class JDemo extends Thread {

    int result;

    public int add(int a, int b) {
        try {
            sleep(1000);
            result = a + b;
        } catch (InterruptedException e) {
        }
        return result;
    }

    public int division(int a, int b) {
        return result = a / b;
    }
}
```

执行结果：

```
in BeforeClass================
in Before
in Test ----Add
in After
in Before
in Test ----Division
in After
in AfterClass=================
```

Junit运行结果，5个成功（1个忽略），1个错误，1个失败。（注意错误和失败不是一回事，错误说明代码有错误，而失败表示该测试方法测试失败）

左下红框中则表示出了各个测试方法的运行状态，可以看到成功、错误、失败、失败各自的图标是不一样的，还可以看到运行时间。
右边部分则是异常堆栈，可查看异常信息。

### 实例总结

1. 参数化测试

有时一个测试方法，不同的参数值会产生不同的结果，那么我们为了测试全面，会把多个参数值都写出来并一一断言测试，这样有时难免费时费力，这是我们便可以采用参数化测试来解决这个问题。参数化测试就好比把一个“输入值，期望值”的集合传入给测试方法，达到一次性测试的目的。

```java
package test;

import static org.junit.Assert.*;

import java.util.Arrays;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.junit.runners.Parameterized;
import org.junit.runners.Parameterized.Parameters;

@RunWith(Parameterized.class)
public class FibonacciTest {

    @Parameters(name = "{index}: fib({0})={1}")
    public static Iterable<Object[]> data() {
        return Arrays.asList(new Object[][] { { 0, 0 }, { 1, 1 }, { 2, 1 },
        { 3, 2 }, { 4, 3 }, { 5, 5 }, { 6, 8 } });
    }

    private int input;
    private int expected;

    public FibonacciTest(int input, int expected) {
        this.input = input;
        this.expected = expected;
    }

    @Test
    public void test() {
        assertEquals(expected, Fibonacci.compute(input));
    }
}

class Fibonacci {

    public static int compute(int input) {
        int result;
        switch (input) {
            case 0:
                result = 0;
                break;
            case 1:
            case 2:
                result = 1;
                break;
            case 3:
                result = 2;
                break;
            case 4:
                result = 3;
                break;
            case 5:
                result = 5;
                break;
            case 6:
                result = 8;
                break;
            default:
                result = 0;
        }
        return result;
    }
}
```

@Parameters注解参数name，实际是测试方法名称。由于一个test()方法就完成了所有测试，那假如某一组测试数据有问题，那在Junit的结果页面里该如何呈现？因此采用name实际上就是区分每个测试数据的测试方法名。如下图：

2. 打包测试

同样，如果一个项目中有很多个测试用例，如果一个个测试也很麻烦，因此打包测试就是一次性测试完成包中含有的所有测试用例。

```java
package test;

import org.junit.runner.RunWith;
import org.junit.runners.Suite;

@RunWith(Suite.class)
@Suite.SuiteClasses({ AssertTests.class, FibonacciTest.class, JDemoTest.class })
public class AllCaseTest {

}
```

这个功能也需要使用一个特殊的Runner ，需要向@RunWith注解传递一个参数Suite.class 。同时，我们还需要另外一个注解@Suite.SuiteClasses，来表明这个类是一个打包测试类。并将需要打包的类作为参数传递给该注解就可以了。至于AllCaseTest随便起一个类名，内容为空既可。运行AllCaseTest类即可完成打包测试

### 3. 异常测试

异常测试与普通断言测试不同，共有三种方法，其中最为灵活的是第三种，可以与断言结合使用

第一种：

```java
@Test(expected= IndexOutOfBoundsException.class) 
public void empty() { 
    new ArrayList<Object>().get(0); 
}
```

第二种：

```java
@Test
public void testExceptionMessage() {
    try {
        new ArrayList<Object>().get(0);
        fail("Expected an IndexOutOfBoundsException to be thrown");
    } catch (IndexOutOfBoundsException anIndexOutOfBoundsException) {
        assertThat(anIndexOutOfBoundsException.getMessage(), is("Index: 0, Size: 0"));
    }
}
```

第三种：

```java
@Rule
public ExpectedException thrown = ExpectedException.none();

@Test
public void shouldTestExceptionMessage() throws IndexOutOfBoundsException {
    List<Object> list = new ArrayList<Object>();

    thrown.expect(IndexOutOfBoundsException.class);
    thrown.expectMessage("Index: 0, Size: 0");
    list.get(0);
    Assert.assertEquals(1, list.get(0));
}
```

在上述几种方法中，无论是expected还是expect都表示期望抛出的异常，假如某一方法，当参数为某一值时会抛出异常，那么使用第一种方法就必须为该参数单独写一个测试方法来测试异常，而无法与其他参数值一同写在一个测试方法里，所以显得累赘。第二种方法虽然解决这个问题，但是写法不仅繁琐也不利于理解。而第三种犯法，不仅能动态更改期望抛出的异常，与断言语句结合的也非常好，因此推荐使用该方法来测试异常。

4. 限时测试

有时为了防止出现死循环或者方法执行过长（或检查方法效率），而需要使用到限时测试。顾名思义，就是超出设定时间即视为测试失败。共有两种写法。

第一种：

```java
@Test(timeout=1000)
public void testWithTimeout() {
    ...
}
```

第二种：

```java
public class HasGlobalTimeout {
    public static String log;

    @Rule
    public Timeout globalTimeout = new Timeout(10000); // 10 seconds max per method tested

    @Test
    public void testInfiniteLoop1() {
        log += "ran1";
        for (;;) {
        }
    }

    @Test
    public void testInfiniteLoop2() {
        log += "ran2";
        for (;;) {
        }
    }
}
```

其中，第二种方法与异常测试的第三种方法的写法类似。也是推荐的写法。

至此，Junit的教程总结性文章已介绍完了。通过系统总结也进一步加深了对Junit的认识，希望也能同样帮助到对Junit还不太理解的朋友。如果大家还有什么好的建议和用法，很欢迎能提出来一起交流。