---
layout: post
title: maven国内镜像配置（Ubuntu）
date: 2014-03-30
author: scsidisk
category: Java
tags: Maven, Java
---

maven在apache的官方镜像非常慢，严重影响速度，建议使用国内的镜像。目前国内的镜像较少，可以使用oschina的镜像，具体配置过程参考：

http://maven.oschina.net/static/help.html

上述安装过程基于Windows环境，以下过程在Ubuntu 12.04下执行。

### maven安装

Ubuntu下使用apt-get安装maven即可：

```
$ sudo apt-get install maven
```

### 配置maven

修改maven的源：

```
$ sudo vim /etc/maven/settings.xml
```

修改以下内容。

在<mirrors>标签中增加以下内容：

```xml
<mirror>
  <id>nexus-osc</id>
  <mirrorOf>*</mirrorOf>
  <name>Nexus osc</name>
  <url>http://maven.oschina.net/content/groups/public/</url>
</mirror>
```

在<profiles>标签中增加以下内容：

```xml
<profile>
  <id>jdk-1.4</id>
  <activation>
    <jdk>1.4</jdk>
  </activation>
  <repositories>
    <repository>
      <id>nexus</id>
      <name>local private nexus</name>
      <url>http://maven.oschina.net/content/groups/public/</url>
      <releases>
        <enabled>true</enabled>
      </releases>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </repository>
  </repositories>
  <pluginRepositories>
    <pluginRepository>
      <id>nexus</id>
      <name>local private nexus</name>
      <url>http://maven.oschina.net/content/groups/public/</url>
      <releases>
        <enabled>true</enabled>
      </releases>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </pluginRepository>
  </pluginRepositories>
</profile>
```

### 复制配置

将该配置文件复制到用户目录，使得每次对maven创建时，都采用该配置：

```
$ cp /etc/maven/settings.xml ~/.m2/
```

这样，通过以上步骤，使用maven编译其他代码时，都是使用的国内镜像服务了。