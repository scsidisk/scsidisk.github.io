---
layout: post
title: "Maven 入门篇"
date: 2014-06-26
author: George Ma
categories: Java
tags: Maven, Java
---

### Maven 是做什么用的？

Maven 是一个项目管理和构建自动化工具。使用惯例优于配置的原则 。它要求在没有定制之前，所有的项目都有如下的结构：

| 目录 | 目的 |
|------|------|
| ${basedir} | 存放 pom.xml和所有的子目录 |
| ${basedir}/src/main/java | 项目的 java源代码 |
| ${basedir}/src/main/resources |项目的资源，比如说 property文件 |
| ${basedir}/src/test/java | 项目的测试类，比如说 JUnit代码 |
| ${basedir}/src/test/resources | 测试使用的资源 |

一个 maven 项目在默认情况下会产生 JAR 文件，另外 ，编译后 的 classes 会放在 ${basedir}/target/classes 下面， JAR 文件会放在 ${basedir}/target 下面。

### Maven 的安装

在安装 maven 前，先保证你安装了 JDK 。 JDK 6 可以从 Oracle 技术网上下载：

http://www.oracle.com/technetwork/cn/java/javase/downloads/index.html。

Maven 官网的下载链接是 : http://maven.apache.org/download.html 。

将下载的包解压到需要安装到的目录。 接下简单配置下环境变量：

```
1、新建环境变量 M2_HOME ,输入值为 Maven 的安装目录。
2、新建环境变量 M2 ，输入值为:  %M2_HOME%\bin  。
3、将 M2 环境变量加入 Path 的最后，如：;%M2%;。
```

下载Maven并解压到你选择的安装目录，例如在windows下的C:\maven，或者Linux下的/usr/local/maven。然后添加系统变量$M2]\_HOME和M2\_HOME/bin到你的系统路径。在终端或者命令提示里输入以下指令:

安装完成后，在命令行运行下面的命令：

```
$ mvn -v
Apache Maven 3.0.3 (r1075438; 2011-03-01 01:31:09+0800)
Maven home: /home/limin/bin/maven3
Java version: 1.6.0_24, vendor: Sun Microsystems Inc.
Java home: /home/limin/bin/jdk1.6.0_24/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "2.6.35-28-generic-pae", arch: "i386", family: "unix"
```

如果你看到类似上面的输出的话，就说明安装成功了。

### 安装所需插件

在终端或者命令提示里输入以下指令:

```
mvn
```

这时，系统会下载maven运行所需的.jar文件到自动生成的默认路径为 ~\.m2\repository的文件夹中。

这个过程大概需要十几分钟。

### 使用国内镜像

http://maven.oschina.net/help.html

### 创建项目

接下来我们用 maven 来建立最著名的“Hello World!”程序 :)

注意：如果你是第一次运行 maven，你需要 Internet 连接，因为 maven 需要从网上下载需要的插件。

我们要做的第一步是建立一个 maven 项目。在 maven 中，我们是执行 maven 目标 (goal) 来做事情的。

maven 目标和 ant 的 target 差不多。在命令行中执行下面的命令来建立我们的 hello world 项目

```
$ mvn archetype:generate -DgroupId=com.mycompany.helloworld -DartifactId=helloworld -Dpackage=com.mycompany.helloworld -Dversion=1.0-SNAPSHOT

或参考maven手册
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

<div style="display:none">
$ mvn archetype:generate -DgroupId=com.xiaomib2c.hbaseTest -DartifactId=hbaseTest -Dpackage=com.xiaomib2c.hbaseTest -Dversion=1.0-SNAPSHOT
$ mvn archetype:generate -DgroupId=com.xiaomib2c.statMonitor -DartifactId=statMonitor -Dpackage=com.xiaomib2c.stat -Dversion=1.0-SNAPSHOT
或者
mvn archetype:create -DgroupId=oschina -DartifactId=simple -DpackageName=net.oschina.simple  -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
</div>

archetype:generate 目标会列出一系列的 archetype 让你选择。 Archetype 可以理解成项目的模型。 Maven 为我们提供了很多种的项目模型，包括从简单的 Swing 到复杂的 Web 应用。我们选择默认的是编号 #16.

```
1: internal -> appfuse-basic-jsf (创建一个基于Hibernate，Spring和JSF的Web应用程序的原型)
2: internal -> appfuse-basic-spring (创建一个基于Hibernate，Spring和Spring MVC的Web应用程序的原型)
3: internal -> appfuse-basic-struts (创建一个基于Hibernate，Spring和Struts 2的Web应用程序的原型)
4: internal -> appfuse-basic-tapestry (创建一个基于Hibernate, Spring 和 Tapestry 4的Web应用程序的原型)
5: internal -> appfuse-core (创建一个基于 Hibernate and Spring 和 XFire的jar应用程序的原型)
6: internal -> appfuse-modular-jsf (创建一个基于 Hibernate，Spring和JSF的模块化应用原型)
7: internal -> appfuse-modular-spring (创建一个基于 Hibernate, Spring 和 Spring MVC 的模块化应用原型)
8: internal -> appfuse-modular-struts (创建一个基于 Hibernate, Spring 和 Struts 2 的模块化应用原型)
9: internal -> appfuse-modular-tapestry (创建一个基于 Hibernate, Spring 和 Tapestry 4 的模块化应用原型)
10: internal -> maven-archetype-j2ee-simple (一个简单的J2EE的Java应用程序)
11: internal -> maven-archetype-marmalade-mojo (一个Maven的 插件开发项目 using marmalade)
12: internal -> maven-archetype-mojo (一个Maven的Java插件开发项目)
13: internal -> maven-archetype-portlet (一个简单的portlet应用程序)
14: internal -> maven-archetype-profiles ()
15: internal -> maven-archetype-quickstart ()
16: internal -> maven-archetype-site-simple (简单的网站生成项目)
17: internal -> maven-archetype-site (更复杂的网站项目)
18: internal -> maven-archetype-webapp (一个简单的Java Web应用程序)
19: internal -> jini-service-archetype (Archetype for Jini service project creation)
20: internal -> softeu-archetype-seam (JSF+Facelets+Seam Archetype)
21: internal -> softeu-archetype-seam-simple (JSF+Facelets+Seam (无残留) 原型)
22: internal -> softeu-archetype-jsf (JSF+Facelets 原型)
23: internal -> jpa-maven-archetype (JPA 应用程序)
24: internal -> spring-osgi-bundle-archetype (Spring-OSGi 原型)
25: internal -> confluence-plugin-archetype (Atlassian 聚合插件原型)
26: internal -> jira-plugin-archetype (Atlassian JIRA 插件原型)
27: internal -> maven-archetype-har (Hibernate 存档)
28: internal -> maven-archetype-sar (JBoss 服务存档)
29: internal -> wicket-archetype-quickstart (一个简单的Apache Wicket的项目)
30: internal -> scala-archetype-simple (一个简单的scala的项目)
31: internal -> lift-archetype-blank (一个 blank/empty liftweb 项目)
32: internal -> lift-archetype-basic (基本（liftweb）项目)
33: internal -> cocoon-22-archetype-block-plain ([http://cocoapacorg2/maven-plugins/])
34: internal -> cocoon-22-archetype-block ([http://cocoapacorg2/maven-plugins/])
35: internal -> cocoon-22-archetype-webapp ([http://cocoapacorg2/maven-plugins/])
36: internal -> myfaces-archetype-helloworld (使用MyFaces的一个简单的原型)
37: internal -> myfaces-archetype-helloworld-facelets (一个使用MyFaces和Facelets的简单原型)
38: internal -> myfaces-archetype-trinidad (一个使用MyFaces和Trinidad的简单原型)
39: internal -> myfaces-archetype-jsfcomponents (一种使用MyFaces创建定制JSF组件的简单的原型)
40: internal -> gmaven-archetype-basic (Groovy的基本原型)
41: internal -> gmaven-archetype-mojo (Groovy mojo 原型)
```

回车确认，就完成了项目的建立.

这时候我们看一下 maven 给我们建立的文件目录结构：

```
../helloworld/
├── pom.xml
└── src
    ├── main
    │   └── java
    │       └── com
    │           └── mycompany
    │               └── helloworld
    │                   └── App.java
    └── test
        └── java
            └── com
                └── mycompany
                    └── helloworld
                        └── AppTest.java

11 directories, 3 files
```

maven 的 archetype 插件建立了一个 helloworld 目录，这个名字来自 artifactId 。在这个目录下面，有一个 Project Object Model(POM) 文件 pom.xml 。这个文件用于描述项目，配置插件和管理依赖关系。
源代码和资源文件放在 src/main 下面，而测试代码和资源放在 src/test 下面。

Maven 已经为我们建立了一个 App.java 文件：

```java
    package com.mycompany.helloworld;

    /**
     * Hello world!
     *
     */
    public class App {


        public static void main( String[] args ) {
            System.out.println( "Hello World!" );
        }
    }
```

 正是我们需要的 Hello World 代码。所以我们可以构建和运行这个程序了。用下面简单的命令构建：

```
$ cd helloworld
$ mvn package
```

maven 将执行以下步骤

1. validate
2. generate-sources
3. process-sources
4. generate-resources
5. process-resources
6. compile


当你第一次运行 maven 的时候，它会从网上的 maven 库 (repository) 下载需要的程序，存放在你电脑的本地库 (local repository) 中，所以这个时候你需要有 Internet 连接。Maven 默认的本地库是 ~/.m2/repository/ ，在 Windows 下是 %USER_HOME%\.m2\repository\ 。

如果构建没有错误的话, maven 在 helloworld 下面建立了一个新的目录 target/ ，构建打包后的 jar 文件 helloworld-1.0-SNAPSHOT.jar 就存放在这个目录下。编译后的 class 文件放在 target/classes/ 目录下面，测试 class 文件放在 target/test-classes/ 目录下面。

 为了验证我们的程序能运行，执行下面的命令：

```
 $ java -cp target/helloworld-1.0-SNAPSHOT.jar com.mycompany.helloworld.App
```

运行成功！！


### Maven 的动作

- validate: 验证项目是正确的和所有必需的信息是可用的
- compile: 编译项目的源代码
- test: 使用合适的单元测试框架测试编译完成的源代码。本测试不需要代码打包或解包。
- package: 把编译的代码合并成发行的格式，如JAR.
- integration-test: 处理并部署包到测试环境进行测试
- verify: 运行检查以验证该包有效和符合质量标准
- install: 安装包到本地仓库，其他项目可以用作本地依赖
- deploy: 在集成和发布环境完成，复制最后包到远程仓库分享给其他的开发者和项目.

还有两个动作

- clean: 清除以前构建的结果
- site: 生成项目的文档站点

生成站点

```
mvn site
```

这个动作会基于 pom 信息生成一个站点，你可以在 target/site 下面看到该文档。

### 生成 Eclipse 项目

可以生成 eclipse 可以导入的项目格式

```
mvn eclipse:eclipse
```

### FAQ

#### No goals have been specified for this build

在pom.xml中配置默认的Goal，如："Goals"，可以是 compile,clean,test等等;

```
<build>
    <defaultGoal>compile</defaultGoal>
</build>
```

或者使用 mvn compile 命令

### 其他命令

mvn clean compile、mvn clean test、mvn clean package、mvn clean install



