---
layout: post
title: Mac 上 Map/Reduce开发环境
date: 2013-07-13
author: metaboy
category: 大数据
tags: [MacOSX,Hadoop,MapReduce]
---

最近，在Mac上折腾了下，想搭建一个hadoop的测试环境，用于写一些Map/Reduce的sample，下面就先将搭建环境的过程记录下来。

1. hadoop 单机搭建

1.1 确认java环境已经安装

在terminal里再次键入"java -version"，出现如下信息：

![](/images/2013/07/mapraduce_on_mac-01.png)

1.2  安装SSH

首先，输入 ssh-keygen -t dsa -P " -f ~/.ssh/id_dsa

ssh-keygen表示生成秘钥；-t表示秘钥类型；-P用于提供密语；-f指定生成的秘钥文件。这个命令在"~/.ssh/"文件夹下创建两个文件id_dsa和id_dsa.pub，是ssh的一对儿私钥和公钥。接下来，将公钥追加到授权的key中去，输入：cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys

ssh免登陆设置完成。然后输入 ssh localhost  查看下即可

1.3  下载hadoop压缩包

1.4   hadoop-env.sh脚本文件，设置如下环境变量：

```
export JAVA_HOME=/usr/alibaba/java
export HADOOP_INSTALL=/Users/metaboy/project/hadoop/hadoop-1.0.4/

export PATH=$PATH:$HADOOP_INSTALL/bin
export HADOOP_OPTS="-Djava.security.krb5.realm=OX.AC.UK -Djava.security.krb5.kdc=kdc0.ox.ac.uk:kdc1.ox.ac.uk"
```

1.5  core-site.xml文件，配置hdfs的地址和端口号：

```
<property>
    <name>fs.default.name</name>
    <value>hdfs://localhost:9000</value>
</property>
<property>
    <name>hadoop.tmp.dir</name>
    <value>/Users/metaboy/project/hadoop/hadoop-1.0.4/tmp</value>
    <description>A base for other temporary directories.</description>
</property>

```

1.6  mapred-site.xml文件，设置map-reduce中jobtracker的地址和端口号：

```
<property>
    <name>mapred.job.tracker</name>
    <value>localhost:9001</value>
</property>
<property>
    <name>mapred.local.dir</name>
    <value>/Users/metaboy/project/hadoop/hadoop-1.0.4/tmp/mapred/local</value>
</property>
<property>
    <name>mapred.system.dir</name>
    <value>/Users/metaboy/project/hadoop/hadoop-1.0.4/tmp/mapred/system</value>
</property>
```

1.7 最后是hdfs-site.xml文件，设置hdfs的默认备份方式：

默认值是3，在伪分布式系统中，需要修改为1。

```
<property>
    <name>dfs.replication</name>
    <value>1</value>
</property>
<property>
    <name>dfs.permissions</name>
    <value>false</value>
</property>
<property>
    <name>dfs.data.dir</name>
    <value>/Users/metaboy/project/hadoop/hadoop-1.0.4/tmp/hdfs/data</value>
</property>
<property>
    <name>dfs.name.dir</name>
    <value>/Users/metaboy/project/hadoop/hadoop-1.0.4/tmp/hdfs/name</value>
</property>
```

1.8 格式化namenode

在terminal里输入如下命令：

```
bin/Hadoop NameNode -format
```

1.9 启动

在terminal里输入如下命令：

```
bin/start-all.sh
```

1.10 测试是否正常

如果一切正常的话，会在http://localhost:50030和http://localhost:50070分别看到map-reduce和hdfs的相关信息。

![](/images/2013/07/mapraduce_on_mac-02.png)

2. hadoop eclipse 插件编译

2.1 导入hadoop的eclipse-plugin的工程

用hadoop提供的文件自行编译，导入hadoop的eclipse-plugin文件夹.打开eclipse，在File菜单中选择Import.

![](/images/2013/07/mapraduce_on_mac-03.png)

2.2 导入后，在项目中新建lib目录，并复制jar文件

在项目中新建lib目录，并将hadoop安装根目录的hadoop-core-1.0.4.jar文件，lib目录中的commons-cli-1.2.jar、commons-configuration-1.6.jar、commons-httpclient-3.0.1.jar、commons-lang-2.4.jar、jackson-core-asl-1.8.8.jar、jackson-mapper-asl-1.8.8.jar文件复制到新建的目录中。复制完成后，将新建目录中的hadoop-core-1.0.4.jar文件名改为hadoop-core.jar 。

![](/images/2013/07/mapraduce_on_mac-04.png)

2.3 修改hadoop-1.0.4/src/contrib目录的build-contrib.xml文件

找到 `<property name="hadoop.root" location="{root}/../../../"/>` ，将location中的路径修改为hadoop-1.0.4的解压路径。

并在该行下面添加以下两行内容。

```
<property name="hadoop.root" location="/Users/metaboy/project/hadoop/hadoop-1.0.4"/>
<property name="eclipse.home" location="/Applications/eclipse"/>
<property name="version" value="1.0.4"/>
```

eclipse.home是指eclipse的安装目录，需要替换为你自己的安装路径。

version的值要替换为hadoop的版本号。

2.4 修改MapReduceTools项目下的build.xml文件

打开build.xml文件，在文件中搜索 `name="jar"`，找到的代码段并加上 `<copyfile >` 列

```
<target name="jar" depends="compile" unless="skip.contrib"]]>
    <mkdirdir="${build.dir}/lib"/>
    <!–<copy file="${hadoop.root}/build/hadoop-core-${version}.jar" tofile="${build.dir}/lib/hadoop-core.jar" verbose="true"/>–>
    <copyfile="${hadoop.root}/hadoop-core-${version}.jar" tofile="${build.dir}/lib/hadoop-core.jar" verbose="true"/>
    <copyfile="${hadoop.root}/lib/commons-cli-1.2.jar" todir="${build.dir}/lib" verbose="true"/>
    <copyfile="${hadoop.root}/lib/commons-lang-2.4.jar" todir="${build.dir}/lib" verbose="true"/>
    <copyfile="${hadoop.root}/lib/commons-configuration-1.6.jar" todir="${build.dir}/lib" verbose="true"/>
    <copyfile="${hadoop.root}/lib/jackson-mapper-asl-1.8.8.jar" todir="${build.dir}/lib" verbose="true"/>
    <copyfile="${hadoop.root}/lib/jackson-core-asl-1.8.8.jar" todir="${build.dir}/lib" verbose="true"/>
    <copyfile="${hadoop.root}/lib/commons-httpclient-3.0.1.jar" todir="${build.dir}/lib" verbose="true"/>
    <!–<copy file="${hadoop.root}/build/ivy/lib/Hadoop/common/commons-cli-${commons-cli.version}.jar" todir="${build.dir}/lib" verbose="true"/>–>
    <jar jarfile="${build.dir}/hadoop-${name}-${version}.jar" manifest="${root}/META-INF/MANIFEST.MF"]]>
        <filesetdir="${build.dir}" includes="classes/ lib/"/>
        <filesetdir="${root}" includes="resources/ plugin.xml"/>
    </jar>
</target>
```

2.5 在build.xml文件中查找 `<import file="../build-contrib.xml"/>` ，在该行下面添加如下内容：

```
<path id="hadoop-jars">
    <fileset dir="${hadoop.root}/">
        <include name="hadoop-*.jar"/>
    </fileset>
</path>
```

2.6 在文件中搜索 `<path refid="eclipse-sdk-jars"/>`，并在该行下添加如下内容：

```
<pathrefid="hadoop-jars"/>
```

2.7 修改 Bundle-ClassPath

使用编辑器打开META-INF文件夹中的MANIFEST.MF文件，在文件中搜索Bundle-ClassPath，可以看到该行内容如下：

```
Bundle-ClassPath: classes/,lib/hadoop-core.jar,lib/commons-cli-1.2.jar,lib/commons-httpclient-3.0.1.jar,lib/jackson-core-asl-1.8.8.jar,lib/jackson-mapper-asl-1.8.8.jar,lib/commons-configuration-1.6.jar,lib/commons-lang-2.4.jar
```
2.8 用ant开始编译

保存build.xml文件后，在build.xml文件上点击右键，并选择 `Run As -> Ant Build`

2.9  安装插件

编译后的plugin文件会保存在 `hadoop-1.0.4/build/contrib/eclipse-plugin` 文件夹中，文件名为 `hadoop-eclipse-plugin-1.0.4.jar` 。

将该文件复制到eclipse安装目录的plugins文件夹下，并重启eclipse。

2.10 测试

在eclipse的window菜单中选择Show View -> Other，在Show View窗口中选择MapReduce Tools下的Map/Reduce Locations，点击OK

![](/images/2013/07/mapraduce_on_mac-05.png)

问题记录：

1. An internal error occurred during: "Connecting to DFS localhost".

详细信息如下：

```
java.lang.NoClassDefFoundError: org/codehaus/jackson/map/JsonMappingException
```

找到刚才添加的lib目录下的几个jar文件，分别解压，然后通过betterzip将这些字节码分别添加到classes下面

![](/images/2013/07/mapraduce_on_mac-06.png)

问题2：Call to localhost/127.0.0.1:9000 failed on connection exception: java.net.ConnectException

使用jps发现NameNode进程没有正确运行。解决的办法就是先停止服务，然后重新格式化namenode。

```
bin/hadoop namenode -format
```

重新启动后，运行该插件无问题

![](/images/2013/07/mapraduce_on_mac-07.png)

问题3：Unable to load realm info from SCDynamicStore

这个问题是hadoop官方的一个bug，详细bug介绍可以参考这个链接：https://issues.apache.org/jira/browse/HADOOP-7489

一般在是当start-all.sh 的时候，会抛出这样的异常：

```
Unable to load realm info from SCDynamicStore
```

网络上有一种解决方法，解决单机部署还行，对于伪分布式部署还是不行：

就是在hadoop-env.sh文件中加上这一行：

```
export HADOOP_OPTS="-Djava.security.krb5.realm=OX.AC.UK -Djava.security.krb5.kdc=kdc0.ox.ac.uk:kdc1.ox.ac.uk"
```

（意思是设置启动hadoop时设定相关的JVM参数）

转自：http://www.wangyuxiong.com/archives/52025

~~~EOF~~~