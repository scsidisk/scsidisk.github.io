---
layout: post
title: Habase 快速测试
date: 2014-07-09
author: scsidisk
category: 大数据
tags: [HBase]
---

本文使用单机模式，了解 HBase 的有关知识。

### 关于ip

本机的 /etc/hosts 里面应该有下面内容

```
127.0.0.1 localhost
```

### 下载 HBase

从 [HBase 下载镜像列表](http://www.apache.org/dyn/closer.cgi/hbase/) 中选择一个比较快的地址。进入 hbase-0.94.21 下载 hbase-0.94.21.tar.gz

```
$ tar xfz hbase-0.94.21.tar.gz
$ cd hbase-0.94.21
$ vim conf/hbase-site.xml
```

编辑为如下内容

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
  <property>
    <name>hbase.rootdir</name>
    <value>file:///DIRECTORY/hbase</value>
  </property>
  <property>
    <name>hbase.zookeeper.property.dataDir</name>
    <value>/DIRECTORY/zookeeper</value>
  </property>
</configuration>
```

### 启动 HBase

```
$ ./bin/start-hbase.sh
starting Master, logging to logs/hbase-user-master-example.org.out
```

### 使用 Shell

启动 Shell

```
$ ./bin/hbase shell
HBase Shell; enter 'help<RETURN>' for list of supported commands.
Type "exit<RETURN>" to leave the HBase Shell
Version: 0.90.0, r1001068, Fri Sep 24 13:55:42 PDT 2010

hbase(main):001:0>
```

执行命令

```
## 查看所有表
hbase(main):003:0> list
## 创建表
hbase(main):003:0> create 'users','user_id','address','info'
## 查看表
hbase(main):003:0> list 'users'
## 写记录
## 注意：最后一个语句没有分号，此时，按回车后将添加数据到表'users'中。
hbase(main):004:0> put 'users','xiaoming','info:age','24';
hbase(main):004:0> put 'users','xiaoming','info:birthday','1987-06-17';
hbase(main):004:0> put 'users','xiaoming','info:company','alibaba';
hbase(main):004:0> put 'users','xiaoming','address:contry','china';
hbase(main):004:0> put 'users','xiaoming','address:province','zhejiang';
hbase(main):004:0> put 'users','xiaoming','address:city','hangzhou';
hbase(main):004:0> put 'users','zhangyifei','info:birthday','1987-4-17';
hbase(main):004:0> put 'users','zhangyifei','info:favorite','movie';
hbase(main):004:0> put 'users','zhangyifei','info:company','alibaba';
hbase(main):004:0> put 'users','zhangyifei','address:contry','china';
hbase(main):004:0> put 'users','zhangyifei','address:province','guangdong';
hbase(main):004:0> put 'users','zhangyifei','address:city','jieyang';
hbase(main):004:0> put 'users','zhangyifei','address:town','xianqiao'
## 扫描记录
hbase(main):007:0> scan 'users'
## 取得一个id的所有数据
hbase(main):008:0> get 'users','xiaoming'
## 获取一个id，一个列族的所有数据
hbase(main):008:0> get 'users','xiaoming','info'
## 获取一个id，一个列族中一个列的所有数据
hbase(main):008:0> get 'users','xiaoming','info:age'
## 更新记录
hbase(main):008:0> put 'users','xiaoming','info:age' ,'29'
hbase(main):008:0> get 'users','xiaoming','info:age'
hbase(main):008:0> put 'users','xiaoming','info:age' ,'30'
hbase(main):008:0> get 'users','xiaoming','info:age'
## 获取单元格数据的版本数据
hbase(main):008:0> get 'users','xiaoming',{COLUMN=>'info:age',VERSIONS=>1}
hbase(main):008:0> get 'users','xiaoming',{COLUMN=>'info:age',VERSIONS=>2}
hbase(main):008:0> get 'users','xiaoming',{COLUMN=>'info:age',VERSIONS=>3}
## 获取单元格数据的某个版本数据
hbase(main):008:0> get 'users','xiaoming',{COLUMN=>'info:age',TIMESTAMP=>1364874937056}
## 删除xiaoming值的'info:age'字段
hbase(main):008:0> delete 'users','xiaoming','info:age'
hbase(main):008:0> get 'users','xiaoming'
## 删除整行
hbase(main):008:0> deleteall 'users','xiaoming'
## 统计表的行数
hbase(main):008:0> count 'users'
## 清空表
hbase(main):008:0> truncate 'users'
## 全表扫描
hbase(main):008:0> scan 'users'
## 停用表
hbase(main):012:0> disable 'users'
## 丢弃表
hbase(main):013:0> drop 'users'
## 退出
hbase(main):014:0> exit
```

### 停止 HBase

```
$ ./bin/stop-hbase.sh
stopping hbase...............
```

### 使用java读取hbase数据

#### maven 生成项目

```
$ mvn archetype:generate -DgroupId=com.mycompany.hbaseTest -DartifactId=hbaseTest -Dpackage=com.mycompany.hbaseTest -Dversion=1.0-SNAPSHOT
$ cd hbaseTest
$ vim pom.xml
## 添加下面内容
--------------------------------------------
<dependency>
	<groupId>org.apache.hbase</groupId>
	<artifactId>hbase</artifactId>
	<version>0.94.7</version>
</dependency>
<dependency>
	<groupId>org.apache.hadoop</groupId>
	<artifactId>hadoop-core</artifactId>
	<version>0.20.2-cdh3u5</version>
	<type>pom</type>
</dependency>
<dependency>
	<groupId>org.apache.thrift</groupId>
	<artifactId>thrift</artifactId>
	<version>0.5.0</version>
</dependency>
<dependency>
	<groupId>com.google.api.client</groupId>
	<artifactId>google-api-client-repackaged-com-google-common-base</artifactId>
	<version>1.2.3-alpha</version>
</dependency>

<dependency>
	<groupId>commons-codec</groupId>
	<artifactId>commons-codec</artifactId>
	<version>20041127.091804</version>
</dependency>
<dependency>
	<groupId>commons-logging</groupId>
	<artifactId>commons-logging</artifactId>
	<version>1.1.3</version>
</dependency>
<dependency>
	<groupId>log4j</groupId>
	<artifactId>log4j</artifactId>
	<version>1.2.17</version>
</dependency>
<dependency>
	<groupId>org.apache.zookeeper</groupId>
	<artifactId>zookeeper</artifactId>
	<version>3.4.6</version>
</dependency>
--------------------------------------------
## 还需要添加
--------------------------------------------
<repositories>
	<repository>
	<id>cloudera</id>
	<url>https://repository.cloudera.com/content/groups/public</url>
	</repository>
</repositories>
<build>
	<defaultGoal>compile</defaultGoal>
</build>
--------------------------------------------
## 下面命令或使用 mvn compile
$ mvn
$ mvn eclipse:eclipse
```

打开 eclipse 导入项目

#### 输出表 users 的列族名称

创建新类 ExampleClient

```java
import java.io.IOException;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.HColumnDescriptor;
import org.apache.hadoop.hbase.HTableDescriptor;
import org.apache.hadoop.hbase.client.HBaseAdmin;
import org.apache.hadoop.hbase.util.Bytes;

public class ExampleClient {
    public static void main(String[] args) throws IOException {

          Configuration conf = HBaseConfiguration.create();
          conf.set("hbase.zookeeper.quorum", "localhost");//使用eclipse时必须添加这个，否则无法定位
          conf.set("hbase.zookeeper.property.clientPort", "2181");
          HBaseAdmin admin = new HBaseAdmin(conf);// 新建一个数据库管理员
          HTableDescriptor tableDescriptor = admin.getTableDescriptor(Bytes.toBytes("users"));
          byte[] name = tableDescriptor.getName();
          System.out.println("result:");

          System.out.println("table name: "+ new String(name));
          HColumnDescriptor[] columnFamilies = tableDescriptor
                  .getColumnFamilies();
          for(HColumnDescriptor d : columnFamilies){
              System.out.println("column Families: "+ d.getNameAsString());
              }
        }
}
```

#### 使用HBase  Java  API对表users2进行操作

创建新类

```java
import java.util.ArrayList;
import java.util.List;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.HColumnDescriptor;
import org.apache.hadoop.hbase.HTableDescriptor;
import org.apache.hadoop.hbase.KeyValue;
import org.apache.hadoop.hbase.client.Delete;
import org.apache.hadoop.hbase.client.Get;
import org.apache.hadoop.hbase.client.HBaseAdmin;
import org.apache.hadoop.hbase.client.HTable;
import org.apache.hadoop.hbase.client.Put;
import org.apache.hadoop.hbase.client.Result;
import org.apache.hadoop.hbase.client.ResultScanner;
import org.apache.hadoop.hbase.client.Scan;
import org.apache.hadoop.hbase.util.Bytes;

public class OperateTable {
    // 声明静态配置
    private static Configuration conf = null;
    static {
        conf = HBaseConfiguration.create();
        conf.set("hbase.zookeeper.quorum", "localhost");
        conf.set("hbase.zookeeper.property.clientPort", "2181");
    }

    // 创建数据库表
    public static void createTable(String tableName, String[] columnFamilys)
            throws Exception {
        // 新建一个数据库管理员
        HBaseAdmin hAdmin = new HBaseAdmin(conf);

        if (hAdmin.tableExists(tableName)) {
            System.out.println("表已经存在");
            System.exit(0);
        } else {
            // 新建一个 scores 表的描述
            HTableDescriptor tableDesc = new HTableDescriptor(tableName);
            // 在描述里添加列族
            for (String columnFamily : columnFamilys) {
                tableDesc.addFamily(new HColumnDescriptor(columnFamily));
            }
            // 根据配置好的描述建表
            hAdmin.createTable(tableDesc);
            System.out.println("创建表成功");
        }
    }

    // 删除数据库表
    public static void deleteTable(String tableName) throws Exception {
        // 新建一个数据库管理员
        HBaseAdmin hAdmin = new HBaseAdmin(conf);

        if (hAdmin.tableExists(tableName)) {
            // 关闭一个表
            hAdmin.disableTable(tableName);
            // 删除一个表
            hAdmin.deleteTable(tableName);
            System.out.println("删除表成功");

        } else {
            System.out.println("删除的表不存在");
            System.exit(0);
        }
    }

    // 添加一条数据
    public static void addRow(String tableName, String row,
            String columnFamily, String column, String value) throws Exception {
        HTable table = new HTable(conf, tableName);
        Put put = new Put(Bytes.toBytes(row));
        // 参数出分别：列族、列、值
        put.add(Bytes.toBytes(columnFamily), Bytes.toBytes(column),
                Bytes.toBytes(value));
        table.put(put);
    }

    // 删除一条数据
    public static void delRow(String tableName, String row) throws Exception {
        HTable table = new HTable(conf, tableName);
        Delete del = new Delete(Bytes.toBytes(row));
        table.delete(del);
    }

    // 删除多条数据
    public static void delMultiRows(String tableName, String[] rows)
            throws Exception {
        HTable table = new HTable(conf, tableName);
        List<Delete> list = new ArrayList<Delete>();

        for (String row : rows) {
            Delete del = new Delete(Bytes.toBytes(row));
            list.add(del);
        }

        table.delete(list);
    }

    // get row
    public static void getRow(String tableName, String row) throws Exception {
        HTable table = new HTable(conf, tableName);
        Get get = new Get(Bytes.toBytes(row));
        Result result = table.get(get);
        // 输出结果
        for (KeyValue rowKV : result.raw()) {
            System.out.print("Row Name: " + new String(rowKV.getRow()) + " ");
            System.out.print("Timestamp: " + rowKV.getTimestamp() + " ");
            System.out.print("column Family: " + new String(rowKV.getFamily()) + " ");
            System.out.print("Row Name:  " + new String(rowKV.getQualifier()) + " ");
            System.out.println("Value: " + new String(rowKV.getValue()) + " ");
        }
    }

    // get all records
    public static void getAllRows(String tableName) throws Exception {
        HTable table = new HTable(conf, tableName);
        Scan scan = new Scan();
        ResultScanner results = table.getScanner(scan);
        // 输出结果
        for (Result result : results) {
            for (KeyValue rowKV : result.raw()) {
                System.out.print("Row Name: " + new String(rowKV.getRow()) + " ");
                System.out.print("Timestamp: " + rowKV.getTimestamp() + " ");
                System.out.print("column Family: " + new String(rowKV.getFamily()) + " ");
                System.out
                        .print("Row Name:  " + new String(rowKV.getQualifier()) + " ");
                System.out.println("Value: " + new String(rowKV.getValue()) + " ");
            }
        }
    }

    // main
    public static void main(String[] args) {
        try {
            String tableName = "users2";

            // 第一步：创建数据库表：“users2”
            String[] columnFamilys = { "info", "course" };
            OperateTable.createTable(tableName, columnFamilys);

            // 第二步：向数据表的添加数据
            // 添加第一行数据
            OperateTable.addRow(tableName, "tht", "info", "age", "20");
            OperateTable.addRow(tableName, "tht", "info", "sex", "boy");
            OperateTable.addRow(tableName, "tht", "course", "china", "97");
            OperateTable.addRow(tableName, "tht", "course", "math", "128");
            OperateTable.addRow(tableName, "tht", "course", "english", "85");
            // 添加第二行数据
            OperateTable.addRow(tableName, "xiaoxue", "info", "age", "19");
            OperateTable.addRow(tableName, "xiaoxue", "info", "sex", "boy");
            OperateTable.addRow(tableName, "xiaoxue", "course", "china", "90");
            OperateTable.addRow(tableName, "xiaoxue", "course", "math", "120");
            OperateTable
                    .addRow(tableName, "xiaoxue", "course", "english", "90");
            // 添加第三行数据
            OperateTable.addRow(tableName, "qingqing", "info", "age", "18");
            OperateTable.addRow(tableName, "qingqing", "info", "sex", "girl");
            OperateTable
                    .addRow(tableName, "qingqing", "course", "china", "100");
            OperateTable.addRow(tableName, "qingqing", "course", "math", "100");
            OperateTable.addRow(tableName, "qingqing", "course", "english",
                    "99");
            // 第三步：获取一条数据
            System.out.println("获取一条数据");
            OperateTable.getRow(tableName, "tht");
            // 第四步：获取所有数据
            System.out.println("获取所有数据");
            OperateTable.getAllRows(tableName);
            // 第五步：删除一条数据
            System.out.println("删除一条数据");
            OperateTable.delRow(tableName, "tht");
            OperateTable.getAllRows(tableName);
            // 第六步：删除多条数据
            System.out.println("删除多条数据");
            String[] rows = { "xiaoxue", "qingqing" };
            OperateTable.delMultiRows(tableName, rows);
            OperateTable.getAllRows(tableName);
            // 第八步：删除数据库
            System.out.println("删除数据库");
            OperateTable.deleteTable(tableName);

        } catch (Exception err) {
            err.printStackTrace();
        }
    }
}
```