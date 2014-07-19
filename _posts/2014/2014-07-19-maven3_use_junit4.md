---
layout: post
title: Maven3中加入Junit4测试
date: 2014-07-19
author: scsidisk
category: Java
tags: Java Maven Junit
---

## 环境

```
Mac OS X 1.9.2
java version "1.6.0_65"
Apache Maven 3.2.1
Junit 4.11
```

## 配置

Maven3 默认的 Junit 是 3.8.1，因为以前用的一直是 Junit4，感觉很不习惯。在  google 中搜一下，还真找到了 Maven 的 Junit4 的插件。http://wiki.unto.net/Maven\_JUnit4\_plugin，不敢独享。

只要在 POM.XML 中加入下面的代码。运行 mvn test 就可以了。

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.11</version>
        <scope>test</scope>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
                <source>1.5</source>
                <target>1.5</target>
            </configuration>
        </plugin>
        <plugin>
            <groupId>net.unto.maven.plugins</groupId>
            <artifactId>maven-junit4-plugin</artifactId>
            <version>1.0-SNAPSHOT</version>
            <executions>
                <execution>
                    <phase>test</phase>
                    <goals>
                        <goal>test</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>

<pluginRepositories>
    <pluginRepository>
        <id>unto.net</id>
        <url>http://repository.unto.net/maven/</url>
        <releases>
            <updatePolicy>daily</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <updatePolicy>daily</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </snapshots>
    </pluginRepository>
</pluginRepositories>
 ```

转自：http://blog.csdn.net/wfnlibo/article/details/1414754