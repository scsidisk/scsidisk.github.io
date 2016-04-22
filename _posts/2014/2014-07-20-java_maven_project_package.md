---
layout: post
title: Maven 项目打包
date: 2014-07-20
author: scsidisk
category: Java
tags: [Java, Maven]
---

打包就是将项目中的各种文件，比如源代码、编译生成的字节码、配置文件、文档，按照规范的格式生成归档，最常见的是JAR包和WAR包.

本文记录一些常用的打包方法，除了JAR包和WAR包，你还能看到如何生成源码包、Javadoc包、以及从命令行可直接运行的CLI包。

## 普通打包

任何一个Maven项目都需要定义POM元素packaging（如果不写则默认值为jar）。该元素决定了项目的打包方式。

默认情况下，maven在做mvn pakage时，只是将项目编译打包到一个jar中，其他的类库则需要引用才行。

如果你不声明该元素，Maven会帮你生成一个JAR包；如果你定义该元素的值为war，那你会得到一个WAR包；如果定义其值为POM（比如是一个父模块），那什么包都不会生成。

在打包war项目的时候排除某些web资源文件，这时就应该配置maven-war-plugin如下：

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-war-plugin</artifactId>
    <version>2.1.1</version>
    <configuration>
      <webResources>
        <resource>
          <directory>src/main/webapp</directory>
          <excludes>
            <exclude>**/*.jpg</exclude>
          </excludes>
        </resource>
      </webResources>
    </configuration>
  </plugin>
```

## 源码包和Javadoc包

源码包和Javadoc包有着广泛的用途，尤其是源码包，当你使用一个第三方依赖的时候，有时候会希望在IDE中直接进入该依赖的源码查看其实现的细节，如果该依赖将源码包发布到了Maven仓库，那么像Eclipse就能通过m2eclipse插件解析下载源码包并关联到你的项目中，十分方便。

### 生成源码包：

```xml
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-source-plugin</artifactId>
    <version>2.1.2</version>
    <executions>
      <execution>
        <id>attach-sources</id>
        <phase>verify</phase>
        <goals>
          <goal>jar-no-fork</goal>
        </goals>
      </execution>
    </executions>
  </plugin>
```

### 生成Javadoc包：

```xml
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-javadoc-plugin</artifactId>
    <version>2.7</version>
    <executions>
      <execution>
        <id>attach-javadocs</id>
          <goals>
            <goal>jar</goal>
          </goals>
      </execution>
    </executions>
  </plugin>
```

为了帮助所有Maven用户更方便的使用Maven中央库中海量的资源，中央仓库的维护者强制要求开源项目提交构件的时候同时提供源码包和Javadoc包。这是个很好的实践，读者也可以尝试在自己所处的公司内部实行，以促进不同项目之间的交流。

## 可执行CLI包

默认Maven生成的JAR包只包含了编译生成的.class文件和项目资源文件，而要得到一个可以直接在命令行通过java命令运行的JAR文件，还要满足两个条件：

- JAR包中的/META-INF/MANIFEST.MF元数据文件必须包含Main-Class信息。
- 项目所有的依赖都必须在Classpath中。

### 使用 maven-shade-plugin 插件

Maven有好几个插件能帮助用户完成上述任务，不过用起来最方便的还是maven-shade-plugin，它可以让用户配置Main-Class的值，然后在打包的时候将值填入/META-INF/MANIFEST.MF文件。关于项目的依赖，它很聪明地将依赖JAR文件全部解压后，再将得到的.class文件连同当前项目的.class文件一起合并到最终的CLI包中，这样，在执行CLI JAR文件的时候，所有需要的类就都在Classpath中了。下面是一个配置样例：

```xml
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <version>1.4</version>
    <executions>
      <execution>
        <phase>package</phase>
        <goals>
          <goal>shade</goal>
        </goals>
        <configuration>
          <transformers>
            <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
              <mainClass>com.juvenxu.mavenbook.HelloWorldCli</mainClass>
            </transformer>
          </transformers>
        </configuration>
      </execution>
    </executions>
  </plugin>
```

上述例子中的，我的Main-Class是com.juvenxu.mavenbook.HelloWorldCli，构建完成后，对应于一个常规的hello-world-1.0.jar文件，我还得到了一个hello-world-1.0-cli.jar文件。

我可以通过java -jar hello-world-1.0-cli.jar命令运行程序。

### 使用 assembly 插件(1)

在pom中 `<build><plugins>` 中添加 assembly 插件

```xml
<plugin>
    <artifactId>maven-assembly-plugin</artifactId>
    <version>2.4</version>
    <configuration>
        <appendAssemblyId>false</appendAssemblyId> <!-- 使用在生成的文件名后面添加id -->
        <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
        </descriptorRefs>
        <archive>
            <manifest>
                <mainClass>com.juvenxu.mavenbook.HelloWorldCli</mainClass> <!-- 主类 -->
            </manifest>
        </archive>
    </configuration>
</plugin>
```

如果允许跳过的测试，需要在 `<build><plugins>` 中添加

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>2.17</version>
    <configuration>
        <skipTests>true</skipTests>
    </configuration>
</plugin>
```

执行mvn assembly:assembly

执行 java -jar target/hello-1.0.jar


### 使用 assembly 插件(2)

下面的配置只要执行 mvn package 即可

```xml
<plugin>
    <artifactId>maven-assembly-plugin</artifactId>
    <configuration>
        <appendAssemblyId>false</appendAssemblyId>
        <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
        </descriptorRefs>
        <archive>
            <manifest>
                <mainClass>com.juvenxu.mavenbook.HelloWorldCli</mainClass>  <!-- 主类 -->
            </manifest>
        </archive>
    </configuration>
    <executions>
        <execution>
            <id>make-assembly</id>
            <phase>package</phase>
            <goals>
                <goal>single</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

## 自定义格式包

实际的软件项目常常会有更复杂的打包需求，例如我们可能需要为客户提供一份产品的分发包，这个包不仅仅包含项目的字节码文件，还得包含依赖以及相关脚本文件以方便客户解压后就能运行，此外分发包还得包含一些必要的文档。这时项目的源码目录结构大致是这样的：

```
pom.xml
src/main/java/
src/main/resources/
src/test/java/
src/test/resources/
src/main/scripts/
src/main/assembly/
README.txt
```

除了基本的pom.xml和一般Maven目录之外，这里还有一个src/main/scripts/目录，该目录会包含一些脚本文件如run.sh和run.bat，src/main/assembly/会包含一个assembly.xml，这是打包的描述文件，稍后介绍，最后的README.txt是份简单的文档。

我们希望最终生成一个zip格式的分发包，它包含如下的一个结构：

```
bin/
lib/
README.txt
```

其中bin/目录包含了可执行脚本run.sh和run.bat，lib/目录包含了项目JAR包和所有依赖JAR，README.txt就是前面提到的文档。

描述清楚需求后，我们就要搬出Maven最强大的打包插件：maven-assembly-plugin。它支持各种打包文件格式，包括zip、tar.gz、tar.bz2等等，通过一个打包描述文件（该例中是src/main/assembly.xml），它能够帮助用户选择具体打包哪些文件集合、依赖、模块、和甚至本地仓库文件，每个项的具体打包路径用户也能自由控制。如下就是对应上述需求的打包描述文件src/main/assembly.xml：

```xml
<assembly>
  <id>bin</id>
  <formats>
    <format>zip</format>
  </formats>
  <dependencySets>
    <dependencySet>
      <useProjectArtifact>true</useProjectArtifact>
      <outputDirectory>lib</outputDirectory>
    </dependencySet>
  </dependencySets>
  <fileSets>
    <fileSet>
      <outputDirectory>/</outputDirectory>
      <includes>
        <include>README.txt</include>
      </includes>
    </fileSet>
    <fileSet>
      <directory>src/main/scripts</directory>
      <outputDirectory>/bin</outputDirectory>
      <includes>
        <include>run.sh</include>
        <include>run.bat</include>
      </includes>
    </fileSet>
  </fileSets>
</assembly>
```

首先这个assembly.xml文件的id对应了其最终生成文件的classifier。

其次formats定义打包生成的文件格式，这里是zip。因此结合id我们会得到一个名为hello-world-1.0-bin.zip的文件。（假设artifactId为hello-world，version为1.0）

dependencySets用来定义选择依赖并定义最终打包到什么目录，这里我们声明的一个depenencySet默认包含所有所有依赖，而useProjectArtifact表示将项目本身生成的构件也包含在内，最终打包至输出包内的lib路径下（由outputDirectory指定）。

fileSets允许用户通过文件或目录的粒度来控制打包。这里的第一个fileSet打包README.txt文件至包的根目录下，第二个fileSet则将src/main/scripts下的run.sh和run.bat文件打包至输出包的bin目录下。

最后，我们需要配置maven-assembly-plugin使用打包描述文件，并绑定生命周期阶段使其自动执行打包操作：

```xml
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-assembly-plugin</artifactId>
    <version>2.2.1</version>
    <configuration>
      <descriptors>
        <descriptor>src/main/assembly/assembly.xml</descriptor>
      </descriptors>
    </configuration>
    <executions>
      <execution>
        <id>make-assembly</id>
        <phase>package</phase>
        <goals>
          <goal>single</goal>
        </goals>
      </execution>
    </executions>
  </plugin>
```

运行mvn clean package之后，我们就能在target/目录下得到名为hello-world-1.0-bin.zip的分发包了。
