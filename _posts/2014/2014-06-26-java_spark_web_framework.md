---
layout: post
title: Java Spark 框架实践
date: 2014-06-26
author: scsidisk
category: 大数据
tags: [Maven, Java]
---

Java Spark - 简单的web框架

## 新建一个web project

```
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-webapp
```


## 加入Eclipse IDE支持

```
mvn eclipse:eclipse -Dwtpversion=2.0
```

## 导入项目

打开Eclipse，Import -> Existing Projects into Workspace

## 添加组件

打开pom.xml，把spark加进去, 需要加一个repository和一个dependency

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>jieqin</groupId>
    <artifactId>blog</artifactId>
    <packaging>war</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>blog Maven Webapp</name>
    <url>http://maven.apache.org</url>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>spark</groupId>
            <artifactId>spark</artifactId>
            <version>0.9.9.4-SNAPSHOT</version>
        </dependency>
    </dependencies>
    <repositories>
        <repository>
            <id>Spark repository</id>
            <url>http://www.sparkjava.com/nexus/content/repositories/spark/</url>
        </repository>
    </repositories>
    <build>
        <finalName>blog</finalName>
    </build>
</project>
```

## 安装组件

```
mvn clean install
```

maven会自动下载spark包，以及spark包所依赖的3个包（jetty-webapp, servlet-api, slf4j-api）

需要jetty的原因是spark使它作为embedded sever，这样不需要额外的server就可以运行spark项目

## 新建一个Java class

```java
package blog;

import spark.*;

public class Test {

    public static void main(String[] args) {
        Spark.get(new Route("/hello") {

            /* (non-Javadoc)
             * @see spark.Route#handle(spark.Request, spark.Response)
             */
            @Override
            public Object handle(Request request, Response response) {
                return "Hello World from Spark!";
            }
        });
    }

}
```

## 运行

注意Eclipse的console view里面出现如下的log

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
== Spark has ignited ...
>> Listening on 0.0.0.0:4567
```

## 测试

这时候表示spark（“火花”的英文）已经被点燃了！打开web浏览器，输入http://localhost:4567/hello

