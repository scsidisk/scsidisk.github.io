http://blog.csdn.net/column/details/yuguiyang-maven.html
http://wiki.jikexueyuan.com/list/java/
http://www.yiibai.com/maven/create-a-web-application-project-with-maven.html#

1. maven 入门
2. maven web simple
3. maven web mysql
4. maven web redis
5. maven web maven-archetype-site

2014-06-26-maven_primer.md
2014-07-19-maven3_use_junit4.md
2014-03-30-maven_domestic_mirroring_configuration.md
2014-07-20-java_maven_project_package.md

Maven学习（一）- 环境搭建
Maven学习（三）- 使用Maven构建Web项目
Maven学习（四）- 使用Maven构建Web项目-测试
Maven学习（五）- 使用Maven构建Struts2项目
Maven学习（六）- 构建Hibernate项目
Maven学习（七）- 构建Spring项目
Maven学习（八）- 构建MyBatis项目
Maven学习（九）- 构建SSH项目
Maven学习（十） - 阶段小结
专栏：Maven学习之旅
Maven深入学习（一）- 坐标
Maven深入学习（二）- 依赖
Maven深入学习（三）- 聚合与继承
Maven深入学习（四）- 知识总结
Maven创建的Web项目无法使用EL表达式
Maven知识点记录 - profile
Maven知识点记录 - repositories
Maven最佳实践：版本管理
Ubuntu上安装Maven3
Maven常用命令-创建Java项目
Maven常用命令-创建Web项目
Maven中引入本地jar包
Maven私服（一） - The nexus service was launched, but failed to start.
Maven私服（二） - Nexus的安装




aliyun阿里云Maven仓库地址——加速你的maven构建
maven仓库用过的人都知道，国内有多么的悲催。还好有比较好用的镜像可以使用，尽快记录下来。速度提升100倍。

http://maven.aliyun.com/nexus/#view-repositories;public~browsestorage

在maven的settings.xml 文件里配置mirrors的子节点，添加如下mirror

    <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>*</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>




关于settings.xml文件，有一点需要注意的是：

$M2_HOME/conf/settings.xml：是全局范围的，整台机器上的所有用户都会直接受到该配置的影响；
~/.m2/settings.xml：是用户范围的，只有当前用户才会受到该配置的影响（若目录下没有此文件，可将上个settings.xml复制一份到本目录下再去修改）