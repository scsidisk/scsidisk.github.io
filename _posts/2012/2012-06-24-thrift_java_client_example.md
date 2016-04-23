---
layout: post
title: "Thrift入门及Java实例演示"
date: 2012-06-24
author: Michael
categories: Java
tags: Java, Maven, Thrift
---

## 一、概述

Thrift是一个软件框架，用来进行可扩展且跨语言的服务的开发。它结合了功能强大的软件堆栈和代码生成引擎，以构建在 C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk, and OCaml 等等编程语言间无缝结合的、高效的服务。

Thrift最初由facebook开发，07年四月开放源码，08年5月进入apache孵化器。thrift允许你定义一个简单的定义文件中的数据类型和服务接口。以作为输入文件，编译器生成代码用来方便地生成RPC客户端和服务器通信的无缝跨编程语言。

官网地址：thrift.apache.org

推荐值得一看的文章：

http://jnb.ociweb.com/jnb/jnbJun2009.html

http://wiki.apache.org/thrift

http://thrift.apache.org/static/files/thrift-20070401.pdf

## 二、下载配置

到官网下载最新版本，截止今日（2012-06-11）最新版本为0.8.0.

1. 如果是Maven构建项目的，直接在pom.xml 中添加如下内容：

```xml
<dependency>
  <groupId>org.apache.thrift</groupId>
  <artifactId>libthrift</artifactId>
  <version>0.8.0</version>
</dependency>
```

也可参考下面信息添加依赖

```xml
<dependency>
  <groupId>org.apache.thrift</groupId>
  <artifactId>libthrift</artifactId>
  <version>0.9.1</version>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-api</artifactId>
  <version>1.7.7</version>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>jcl-over-slf4j</artifactId>
  <version>1.7.7</version>
  <scope>runtime</scope>
</dependency>

<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>1.5.8</version>
</dependency>
```

如果提示找不到jar包，需要手动添加jar包到 Class Path

2.如果自己编译lib包，把下载的压缩包解压到X:盘，然后在X:\thrift-0.8.0\lib\java 目录下运行ant进行自动编译，会在X:\thrift-0.8.0\lib\java\build\ 目录下看到编译好的lib包：libthrift-0.8.0.jar

## 三、基本概念

### 1.数据类型

#### 基本类型：

```
bool：布尔值，true 或 false，对应 Java 的 boolean
byte：8 位有符号整数，对应 Java 的 byte
i16：16 位有符号整数，对应 Java 的 short
i32：32 位有符号整数，对应 Java 的 int
i64：64 位有符号整数，对应 Java 的 long
double：64 位浮点数，对应 Java 的 double
string：utf-8编码的字符串，对应 Java 的 String
```

#### 结构体类型：

```
struct：定义公共的对象，类似于 C 语言中的结构体定义，在 Java 中是一个 JavaBean
```

#### 容器类型：

```
list：对应 Java 的 ArrayList
set：对应 Java 的 HashSet
map：对应 Java 的 HashMap
```

#### 异常类型：

```
exception：对应 Java 的 Exception
```

#### 服务类型：

```
service：对应服务的类
```

### 2.服务端编码基本步骤：

- 实现服务处理接口impl
- 创建TProcessor
- 创建TServerTransport
- 创建TProtocol
- 创建TServer
- 启动Server

### 3.客户端编码基本步骤：

- 创建Transport
- 创建TProtocol
- 基于TTransport和TProtocol创建 Client
- 调用Client的相应方法

### 4.数据传输协议

- TBinaryProtocol : 二进制格式.
- TCompactProtocol : 压缩格式
- TJSONProtocol : JSON格式
- TSimpleJSONProtocol : 提供JSON只写协议, 生成的文件很容易通过脚本语言解析

提示:客户端和服务端的协议要一致

## 四、实例演示

### 1. thrift生成代码

创建Thrift文件：G:\test\thrift\demoHello.thrift ,内容如下：

```
namespace java com.micmiu.thrift.demo

service  HelloWorldService {
  string sayHello(1:string username)
}
```

目录结构如下：

```
G:\test\thrift>tree /F
卷 other 的文件夹 PATH 列表
卷序列号为 D238-BEG:.
    demoHello.thrift
    demouser.thrift
    thrift-0.8.0.exe
```

thrift-0.8.0.exe -r -gen java ./demoHello.thrift

生成后的目录结构如下：

```
G:\test\thrift>tree /F
卷 other 的文件夹 PATH 列表
卷序列号为 D238-BEG:.
│  demoHello.thrift
│  demouser.thrift
│  thrift-0.8.0.exe
│
└─gen-java
    └─com
        └─micmiu
            └─thrift
                └─demo
                        HelloWorldService.java
```

将生成的HelloWorldService.java 文件copy到自己测试的工程中，我的工程是用maven构建的，故在pom.xml中增加如下内容：

```xml
<dependency>
    <groupId>org.apache.thrift</groupId>
    <artifactId>libthrift</artifactId>
    <version>0.8.0</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.5.8</version>
</dependency>
```

### 2. 实现接口Iface

java代码：HelloWorldImpl.java

```java
package com.micmiu.thrift.demo;

import org.apache.thrift.TException;

/**
 * blog http://www.micmiu.com
 *
 * @author Michael
 *
 */
public class HelloWorldImpl implements HelloWorldService.Iface {

    public HelloWorldImpl() {
    }

    @Override
    public String sayHello(String username) throws TException {
        return "Hi," + username + " welcome to my blog www.micmiu.com";
    }

}
```

### 3.TSimpleServer服务端

简单的单线程服务模型，一般用于测试。

编写服务端server代码：HelloServerDemo.java

```java
package com.micmiu.thrift.demo;

import org.apache.thrift.TProcessor;
import org.apache.thrift.protocol.TBinaryProtocol;
import org.apache.thrift.protocol.TCompactProtocol;
import org.apache.thrift.protocol.TJSONProtocol;
import org.apache.thrift.protocol.TSimpleJSONProtocol;
import org.apache.thrift.server.TServer;
import org.apache.thrift.server.TSimpleServer;
import org.apache.thrift.transport.TServerSocket;

/**
 * blog http://www.micmiu.com
 *
 * @author Michael
 *
 */
public class HelloServerDemo {
    public static final int SERVER_PORT = 8090;

    public void startServer() {
        try {
            System.out.println("HelloWorld TSimpleServer start ....");

            TProcessor tprocessor = new HelloWorldService.Processor<HelloWorldService.Iface>(
                    new HelloWorldImpl());
            // HelloWorldService.Processor<HelloWorldService.Iface> tprocessor =
            // new HelloWorldService.Processor<HelloWorldService.Iface>(
            // new HelloWorldImpl());

            // 简单的单线程服务模型，一般用于测试
            TServerSocket serverTransport = new TServerSocket(SERVER_PORT);
            TServer.Args tArgs = new TServer.Args(serverTransport);
            tArgs.processor(tprocessor);
            tArgs.protocolFactory(new TBinaryProtocol.Factory());
            // tArgs.protocolFactory(new TCompactProtocol.Factory());
            // tArgs.protocolFactory(new TJSONProtocol.Factory());
            TServer server = new TSimpleServer(tArgs);
            server.serve();

        } catch (Exception e) {
            System.out.println("Server start error!!!");
            e.printStackTrace();
        }
    }

    /**
     * @param args
     */
    public static void main(String]args) {
        HelloServerDemo server = new HelloServerDemo();
        server.startServer();
    }

}
```

编写客户端Client代码：HelloClientDemo.java

```java
package com.micmiu.thrift.demo;

import org.apache.thrift.TException;
import org.apache.thrift.protocol.TBinaryProtocol;
import org.apache.thrift.protocol.TCompactProtocol;
import org.apache.thrift.protocol.TJSONProtocol;
import org.apache.thrift.protocol.TProtocol;
import org.apache.thrift.transport.TSocket;
import org.apache.thrift.transport.TTransport;
import org.apache.thrift.transport.TTransportException;

/**
 * blog http://www.micmiu.com
 *
 * @author Michael
 *
 */
public class HelloClientDemo {

    public static final String SERVER_IP = "localhost";
    public static final int SERVER_PORT = 8090;
    public static final int TIMEOUT = 30000;

    /**
     *
     * @param userName
     */
    public void startClient(String userName) {
        TTransport transport = null;
        try {
            transport = new TSocket(SERVER_IP, SERVER_PORT, TIMEOUT);
            // 协议要和服务端一致
            TProtocol protocol = new TBinaryProtocol(transport);
            // TProtocol protocol = new TCompactProtocol(transport);
            // TProtocol protocol = new TJSONProtocol(transport);
            HelloWorldService.Client client = new HelloWorldService.Client(
                    protocol);
            transport.open();
            String result = client.sayHello(userName);
            System.out.println("Thrify client result =: " + result);
        } catch (TTransportException e) {
            e.printStackTrace();
        } catch (TException e) {
            e.printStackTrace();
        } finally {
            if (null != transport) {
                transport.close();
            }
        }
    }

    /**
     * @param args
     */
    public static void main(String]args) {
        HelloClientDemo client = new HelloClientDemo();
        client.startClient("Michael");

    }

}
```

先运行服务端程序，日志如下：

```
HelloWorld TSimpleServer start ....
```

再运行客户端调用程序，日志如下：

```
Thrify client result =: Hi,Michael welcome to my blog www.micmiu.com
```

测试成功，和预期的返回信息一致。

### 4.TThreadPoolServer 服务模型

线程池服务模型，使用标准的阻塞式IO，预先创建一组线程处理请求。

编写服务端代码：HelloServerDemo.java

```java
package com.micmiu.thrift.demo;

import org.apache.thrift.TProcessor;
import org.apache.thrift.protocol.TBinaryProtocol;
import org.apache.thrift.server.TServer;
import org.apache.thrift.server.TThreadPoolServer;
import org.apache.thrift.transport.TServerSocket;

/**
 * blog http://www.micmiu.com
 *
 * @author Michael
 *
 */
public class HelloServerDemo {
    public static final int SERVER_PORT = 8090;

    public void startServer() {
        try {
            System.out.println("HelloWorld TThreadPoolServer start ....");

            TProcessor tprocessor = new HelloWorldService.Processor<HelloWorldService.Iface>(
                    new HelloWorldImpl());

             TServerSocket serverTransport = new TServerSocket(SERVER_PORT);
             TThreadPoolServer.Args ttpsArgs = new TThreadPoolServer.Args(
             serverTransport);
             ttpsArgs.processor(tprocessor);
             ttpsArgs.protocolFactory(new TBinaryProtocol.Factory());

            // 线程池服务模型，使用标准的阻塞式IO，预先创建一组线程处理请求。
             TServer server = new TThreadPoolServer(ttpsArgs);
             server.serve();

        } catch (Exception e) {
            System.out.println("Server start error!!!");
            e.printStackTrace();
        }
    }

    /**
     * @param args
     */
    public static void main(String]args) {
        HelloServerDemo server = new HelloServerDemo();
        server.startServer();
    }

}
```

客户端Client代码和之前的一样，只要数据传输的协议一致即可，客户端测试成功，结果如下：

```
Thrify client result =: Hi,Michael welcome to my blog www.micmiu.com
```

### 5.TNonblockingServer 服务模型

使用非阻塞式IO，服务端和客户端需要指定 TFramedTransport 数据传输的方式。

编写服务端代码：HelloServerDemo.java

```java
package com.micmiu.thrift.demo;

import org.apache.thrift.TProcessor;
import org.apache.thrift.protocol.TCompactProtocol;
import org.apache.thrift.server.TNonblockingServer;
import org.apache.thrift.server.TServer;
import org.apache.thrift.transport.TFramedTransport;
import org.apache.thrift.transport.TNonblockingServerSocket;

/**
 * blog http://www.micmiu.com
 *
 * @author Michael
 *
 */
public class HelloServerDemo {
    public static final int SERVER_PORT = 8090;

    public void startServer() {
        try {
            System.out.println("HelloWorld TNonblockingServer start ....");

            TProcessor tprocessor = new HelloWorldService.Processor<HelloWorldService.Iface>(
                    new HelloWorldImpl());

            TNonblockingServerSocket tnbSocketTransport = new TNonblockingServerSocket(
                    SERVER_PORT);
            TNonblockingServer.Args tnbArgs = new TNonblockingServer.Args(
                    tnbSocketTransport);
            tnbArgs.processor(tprocessor);
            tnbArgs.transportFactory(new TFramedTransport.Factory());
            tnbArgs.protocolFactory(new TCompactProtocol.Factory());

            // 使用非阻塞式IO，服务端和客户端需要指定TFramedTransport数据传输的方式
            TServer server = new TNonblockingServer(tnbArgs);
            server.serve();

        } catch (Exception e) {
            System.out.println("Server start error!!!");
            e.printStackTrace();
        }
    }

    /**
     * @param args
     */
    public static void main(String]args) {
        HelloServerDemo server = new HelloServerDemo();
        server.startServer();
    }

}
```

编写客户端代码：HelloClientDemo.java

```java
package com.micmiu.thrift.demo;

import org.apache.thrift.TException;
import org.apache.thrift.protocol.TCompactProtocol;
import org.apache.thrift.protocol.TProtocol;
import org.apache.thrift.transport.TFramedTransport;
import org.apache.thrift.transport.TSocket;
import org.apache.thrift.transport.TTransport;
import org.apache.thrift.transport.TTransportException;

/**
 * blog http://www.micmiu.com
 *
 * @author Michael
 *
 */
public class HelloClientDemo {

    public static final String SERVER_IP = "localhost";
    public static final int SERVER_PORT = 8090;
    public static final int TIMEOUT = 30000;

    /**
     *
     * @param userName
     */
    public void startClient(String userName) {
        TTransport transport = null;
        try {
            transport = new TFramedTransport(new TSocket(SERVER_IP,
                    SERVER_PORT, TIMEOUT));
            // 协议要和服务端一致
            TProtocol protocol = new TCompactProtocol(transport);
            HelloWorldService.Client client = new HelloWorldService.Client(
                    protocol);
            transport.open();
            String result = client.sayHello(userName);
            System.out.println("Thrify client result =: " + result);
        } catch (TTransportException e) {
            e.printStackTrace();
        } catch (TException e) {
            e.printStackTrace();
        } finally {
            if (null != transport) {
                transport.close();
            }
        }
    }

    /**
     * @param args
     */
    public static void main(String]args) {
        HelloClientDemo client = new HelloClientDemo();
        client.startClient("Michael");

    }

}
```

客户端的测试成功，结果如下：

```
Thrify client result =: Hi,Michael welcome to my blog www.micmiu.com
```

### 6.THsHaServer服务模型

半同步半异步的服务端模型，需要指定为： TFramedTransport 数据传输的方式。

编写服务端代码：HelloServerDemo.java

```java
package com.micmiu.thrift.demo;

import org.apache.thrift.TProcessor;
import org.apache.thrift.protocol.TBinaryProtocol;
import org.apache.thrift.protocol.TCompactProtocol;
import org.apache.thrift.server.THsHaServer;
import org.apache.thrift.server.TNonblockingServer;
import org.apache.thrift.server.TServer;
import org.apache.thrift.server.TSimpleServer;
import org.apache.thrift.server.TThreadPoolServer;
import org.apache.thrift.transport.TFramedTransport;
import org.apache.thrift.transport.TNonblockingServerSocket;
import org.apache.thrift.transport.TServerSocket;

/**
 * blog http://www.micmiu.com
 *
 * @author Michael
 *
 */
public class HelloServerDemo {
    public static final int SERVER_PORT = 8090;

    public void startServer() {
        try {
            System.out.println("HelloWorld THsHaServer start ....");

            TProcessor tprocessor = new HelloWorldService.Processor<HelloWorldService.Iface>(
                    new HelloWorldImpl());

            TNonblockingServerSocket tnbSocketTransport = new TNonblockingServerSocket(
                    SERVER_PORT);
            THsHaServer.Args thhsArgs = new THsHaServer.Args(tnbSocketTransport);
            thhsArgs.processor(tprocessor);
            thhsArgs.transportFactory(new TFramedTransport.Factory());
            thhsArgs.protocolFactory(new TBinaryProtocol.Factory());

            //半同步半异步的服务模型
            TServer server = new THsHaServer(thhsArgs);
            server.serve();

        } catch (Exception e) {
            System.out.println("Server start error!!!");
            e.printStackTrace();
        }
    }

    /**
     * @param args
     */
    public static void main(String]args) {
        HelloServerDemo server = new HelloServerDemo();
        server.startServer();
    }

}
```

客户端代码和上面 4 中的类似，只要注意传输协议一致以及指定传输方式为TFramedTransport。

### 7.异步客户端

编写服务端代码：HelloServerDemo.java

```java
package com.micmiu.thrift.demo;

import org.apache.thrift.TProcessor;
import org.apache.thrift.protocol.TCompactProtocol;
import org.apache.thrift.server.TNonblockingServer;
import org.apache.thrift.server.TServer;
import org.apache.thrift.transport.TFramedTransport;
import org.apache.thrift.transport.TNonblockingServerSocket;

/**
 * blog http://www.micmiu.com
 *
 * @author Michael
 *
 */
public class HelloServerDemo {
    public static final int SERVER_PORT = 8090;

    public void startServer() {
        try {
            System.out.println("HelloWorld TNonblockingServer start ....");

            TProcessor tprocessor = new HelloWorldService.Processor<HelloWorldService.Iface>(
                    new HelloWorldImpl());

            TNonblockingServerSocket tnbSocketTransport = new TNonblockingServerSocket(
                    SERVER_PORT);
            TNonblockingServer.Args tnbArgs = new TNonblockingServer.Args(
                    tnbSocketTransport);
            tnbArgs.processor(tprocessor);
            tnbArgs.transportFactory(new TFramedTransport.Factory());
            tnbArgs.protocolFactory(new TCompactProtocol.Factory());

            // 使用非阻塞式IO，服务端和客户端需要指定TFramedTransport数据传输的方式
            TServer server = new TNonblockingServer(tnbArgs);
            server.serve();

        } catch (Exception e) {
            System.out.println("Server start error!!!");
            e.printStackTrace();
        }
    }

    /**
     * @param args
     */
    public static void main(String]args) {
        HelloServerDemo server = new HelloServerDemo();
        server.startServer();
    }

}
```

编写客户端Client代码：HelloAsynClientDemo.java

```java
package com.micmiu.thrift.demo;

import java.util.concurrent.CountDownLatch;
import java.util.concurrent.TimeUnit;

import org.apache.thrift.TException;
import org.apache.thrift.async.AsyncMethodCallback;
import org.apache.thrift.async.TAsyncClientManager;
import org.apache.thrift.protocol.TCompactProtocol;
import org.apache.thrift.protocol.TProtocolFactory;
import org.apache.thrift.transport.TNonblockingSocket;
import org.apache.thrift.transport.TNonblockingTransport;

import com.micmiu.thrift.demo.HelloWorldService.AsyncClient.sayHello_call;

/**
 * blog http://www.micmiu.com
 *
 * @author Michael
 *
 */
public class HelloAsynClientDemo {

    public static final String SERVER_IP = "localhost";
    public static final int SERVER_PORT = 8090;
    public static final int TIMEOUT = 30000;

    /**
     *
     * @param userName
     */
    public void startClient(String userName) {
        try {
            TAsyncClientManager clientManager = new TAsyncClientManager();
            TNonblockingTransport transport = new TNonblockingSocket(SERVER_IP,
                    SERVER_PORT, TIMEOUT);

            TProtocolFactory tprotocol = new TCompactProtocol.Factory();
            HelloWorldService.AsyncClient asyncClient = new HelloWorldService.AsyncClient(
                    tprotocol, clientManager, transport);
            System.out.println("Client start .....");

            CountDownLatch latch = new CountDownLatch(1);
            AsynCallback callBack = new AsynCallback(latch);
            System.out.println("call method sayHello start ...");
            asyncClient.sayHello(userName, callBack);
            System.out.println("call method sayHello .... end");
            boolean wait = latch.await(30, TimeUnit.SECONDS);
            System.out.println("latch.await =:" + wait);
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println("startClient end.");
    }

    public class AsynCallback implements AsyncMethodCallback<sayHello_call> {
        private CountDownLatch latch;

        public AsynCallback(CountDownLatch latch) {
            this.latch = latch;
        }

        @Override
        public void onComplete(sayHello_call response) {
            System.out.println("onComplete");
            try {
                // Thread.sleep(1000L * 1);
                System.out.println("AsynCall result =:"
                        + response.getResult().toString());
            } catch (TException e) {
                e.printStackTrace();
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                latch.countDown();
            }
        }

        @Override
        public void onError(Exception exception) {
            System.out.println("onError :" + exception.getMessage());
            latch.countDown();
        }
    }

    /**
     * @param args
     */
    public static void main(String]args) {
        HelloAsynClientDemo client = new HelloAsynClientDemo();
        client.startClient("Michael");

    }

}
```

先运行服务程序，再运行客户端程序，测试结果如下：

```
Client start .....
call method sayHello start ...
call method sayHello .... end
onComplete
AsynCall result =:Hi,Michael welcome to my blog www.micmiu.com
latch.await =:true
startClient end.
```