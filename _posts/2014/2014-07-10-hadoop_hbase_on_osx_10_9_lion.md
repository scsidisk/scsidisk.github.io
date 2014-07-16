---
layout: post
title: Hadoop & Hbase on OSX Lion
date: 2014-07-10
author: scsidisk
category: HBase
tags: HBase
---

Hadoop & Hbase on OSX 10.8 Mountain Lion

原文地址：http://freddy.cellcore.org/post/52568231952/hadoop-hbase-on-osx-10-8-mountain-lion

A lot of friends are currently using or planning to use big data technologies, particularlyHadoop. Hadoop and Hbase are your companion if you need to crunch large amounts of data in short time and without throwing out a lot of resources (a single box with lots of processing power).

Ideally, you will need a cluster of commodity hardware (cheap) servers, but for development and testing purposes a local instance of Hadoop will do better than a remote server cluster.

This recipe will take you about 15 minutes to have a Hadoop and Hbase instance up and running.

> I’m assuming that you know what Hadoop and Hbase are, and that you need to install a development instance of both.


### Pre-requisites and Assumptions

I’m assuming that you are installing Hadoop and Hbase on a box running OSX (I’m running OSX 10.8 mountain lion, but I don’t see a reason why this should not work the same way for other versions of OSX).

I’m also assuming that you are using your regular user (in my case “freddy”), that your home directory is ``/Users/USER`` (refered as ``~`` or ``$HOME``) and your Hadoop and Hbase installation directory are respectively ``~/tools/hd/hadoop`` and ``~/tools/hd/hbase``.

If you don’t have a ``~/tools/hd`` directory you can create it right new by executing the command. ``mkdir -p ~/tools/hd``.

For the sake of simplicity, in the reminder of this recipe I will use the shell variables ``$HD_HOME``,``$HADOOP_HOME`` and ``$HBASE_HOME`` to identify the ``Hadoop/Hbase`` installation directory.

```
export HD_HOME=~/tools/hd
export HADOOP_HOME=~/tools/hd/hadoop
export HBASE_HOME=~/tools/hd/hbase
```

There are two basic requirements that you need to complete before installing Hadoop and Hbase, these are Java and ssh.

### Java

Hadoop and Hbase are developed using Java, and therefore to run them you should have Java installed on your system. Verify that you have Java 1.6 or higher installed on your system (by default in OSX 10.8) by executing:

```
java -version
```

This should produce an output similar to:

```
java version "1.6.0_45"
Java(TM) SE Runtime Environment (build 1.6.0_45-b06-451-11M4406)
Java HotSpot(TM) 64-Bit Server VM (build 20.45-b01-451, mixed mode)
```

If you don’t have Java installed in your system, entering the previous command will prompt its installation (OSX 10.8+ only).

### ssh

Hadoop manages all its nodes using ssh (yes, event a single node cluster). To enable ssh (Remote Login) on OSX 10.8 you should execute the following command:

```
sudo systemsetup -setremotelogin on
```

You will also need to generate a ssh public/private keypair in order to allow Hadoop to connect password-less into your node(s). To do so, execute the following command:

```
ssh-keygen -t rsa -P ""
```

This will produce an output similar to:

```
Generating public/private rsa key pair.
Enter file in which to save the key (Users/freddy/.ssh/id_rsa): id_rsa
Your identification has been saved in id_rsa_hbase.
Your public key has been saved in id_rsa_hbase.pub.
The key fingerprint is:
46:a9:26:fc:16:c8:ca:f4:64:37:cd:8c:c6:8d:41:3a freddy@chronos.home
The key's randomart image is:
+--[ RSA 2048]----+
|      .          |
|     o   .       |
|    E . o        |
|   o + @         |
|  . B @ S        |
| o = * +         |
|  o . o          |
|     .           |
|                 |
+-----------------+
```

Now install the key into the authorized keys:

```
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

And try to log into localhost (ssh should not ask for a connection password).

```
ssh localhost
```

> If you already have a default ssh keys in your computer (otherwise ignore this alert), you may want to change the name of the generated key. For example enterid_rsa_hbase instead of id_rsa . 
> To tell ssh to use this particular key when connecting to localhost, edit the file~/.ssh/config and add the following lines:
> host localhost
>>      user freddy
>>      identityfile ~/.ssh/id_rsa_hbase
    
### Hadoop

#### Downloading Hadoop

Now, your system is ready to run Hadoop so you can proceed to download it. To do so, you can either go to the Hadoop distribution site, choose a mirror close to your location and download it (then copu to ``$HD_HOME``), or execute the following commands:

```
cd ~/Downloads
curl http://mir2.ovh.net/ftp.apache.org/dist/hadoop/common/stable/hadoop-1.1.2.tar.gz > hadoop-1.1.2.tar.gz
mv hadoop-1.1.2.tar.gz $HD_HOME/
cd $HD_HOME
tar xvzf hadoop-1.1.2.tar.gz
ln -s hadoop-1.1.2 hadoop
```

This will download and install the Hadoop files into ``$HADOOP_HOME``.

#### Configuring Hadoop

In order to set up your single node cluster, you will need to modify a series of configuration files (all located under ``$HADOOP_HOME/conf/``).

##### hadoop-env.sh

The first configuration files to modify is ``hadoop-env.sh``. This file contains all the “shell” environment variables that Hadoop requires to work properly. So fire your prefer text editor and open ``$HADOOP_HOME/conf/hadoop-env.sh``. Add the lines below to your file.

- JAVA_HOME=/System/Library/Frameworks/JavaVM.framework/Versions/CurrentJDK/Home
- HADOOP_OPTS="-Djava.net.preferIPv4Stack=true -Djava.security.krb5.realm=OX.AC.UK -Djava.security.krb5.kdc=kdc0.ox.ac.uk:kdc1.ox.ac.uk" 

> The property ``-Djava.net.preferIPv4Stack=true`` will force Hadoop components to use IPv4. This is requires since Hadoop does not work pretty well with IPv6. You can find more about this here and here.

> The property ``-Djava.security.krb5.realm= -Djava.security.krb5.kdc=`` indicates Hadoop to avoid kerberos realm. Without this line you will get the following warning ``Unable to load realm info from SCDynamicStore``. You can find more about thishere.

##### core-site.xml

The next file you want to modify is ``core-site.xml``. This file contains the base configuration of Hadoop distributed file system (HDFS). The property ``fs.default.name`` indicates the host and port where the Hadoop name node is be located. The property hadoop.tmp.dir indicates Hadoop where it should write its distributed systems temporal (and non-temporal) files (in this case $HD_HOME/hadoopdata).

```xml
<configuration>
  <property>
    <name>fs.default.name</name>
    <value>hdfs://localhost:9000</value>
  </property>
  <property>
    <name>hadoop.tmp.dir</name>
    <value>/Users/freddy/tools/hd/hadoopdata</value>
    <description>A base for other temporary directories.</description>
  </property>
</configuration>
```

> Hadoop must be able to write to the ``hadoop.tmp.dir`` directory to work.

##### hdfs-site.xml

HDFS uses a replication procedure to ensure fault tolerance (and avoid losing data). The configuration file hdfs-site.xml contains the parameters that rule this “replication” (a list of other properties that can be included in this file can be found here). Notice that since we have only a single node, it’s fair to have only one copy of each file.

```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```

##### mapred-site.xml

Hadoop enables parallel processing using MapReduce. The Hadoop MapReduce instance is manager by the property file ``mapred-site.xml``. Here you can set several properties, but to make MapReduce work only a few are requires. Set ``mapred.job.tracker`` to indicate the host and port where the job tracker is located (the “task manager”).``mapred.tasktracker.map.tasks.maximum`` and ``mapred.tasktracker.reduce.tasks.maximum`` indicate the maximum number of map and reduce tasks that can be executed.

As a rule of thumb, you should not have map tasks than the number of cores in your cpu (single node cluster), and one reduce for each physical disk you have. In my case I have four cores and one disk.

```xml
<configuration>
  <property>
    <name>mapred.job.tracker</name>
    <value>localhost:9001</value>
  </property>
  <property>
    <name>mapred.tasktracker.map.tasks.maximum</name>
    <value>4</value>
  </property>
  <property>
    <name>mapred.tasktracker.reduce.tasks.maximum</name>
    <value>1</value>
  </property>
</configuration>
```

#### Running Hadoop

##### Format your name node

Before you launch Hadoop, HDFS requires to be initialized (formatted). To format your HDFS name node execute the following command:

```
$HADOOP_HOME/bin/hadoop namenode -format
```

You will get a very verbose output from this command, verify that you have a line similar to:

```
...
13/06/09 19:18:51 INFO common.Storage: Storage directory /Users/freddy/tools/hd/hadoopdata/dfs/name has been successfully formatted.
...
```

##### Launch hadoop

Your Hadoop instance is configured and ready to be launched.

```
$ $HADOOP_HOME/bin/start-all.sh
```

A command that comes at hand to check what (java) services are running is ``jps``. If you execute it on your console you should see an output similar to:

```
5032 JobTracker
4813 NameNode
4969 SecondaryNameNode
4891 DataNode
5110 TaskTracker
79915 Jps
```

If JobTracker, NameNode, SecondaryNameNode, DataNode, and TaskTracker are there, then Hadoop is running.

##### Test it

Note that HDFS is not a filesystem layer but a “virtual” file system that provides a very particular API. Try to browse the filesystem root:

```
$HADOOP_HOME/bin/hadoop dfs -ls /
```

create a new folder:

```
$HADOOP_HOME/bin/hadoop dfs -mkdir /test
drwxr-xr-x   - freddy supergroup          0 2013-06-09 19:26 /test
```

copy a file:

``` 
echo "test file" > /tmp/tempFileTest
$HADOOP_HOME/bin/hadoop dfs -copyFromLocal /tmp/tempFileTest /test
rm /tmp/tempFileTest
```

and check the file content.

``` 
$HADOOP_HOME/bin/hadoop dfs -ls /test
-rw-r--r--   1 freddy supergroup          0 2013-06-09 19:30 /test/tempFileTest
$HADOOP_HOME/bin/hadoop dfs -cat  /test/tempFileTest
"test file"
```

You may also want to check that your brand new MapReduce setup is working.

```
$HADOOP_HOME/bin/hadoop jar hadoop-examples-1.1.2.jar pi 10 50
```

This job should output something similar to:

```
Job Finished in 20.456 seconds
Estimated value of Pi is 3.16000000000000000000
```

##### Hadoop web-interfaces

Hadoop comes with a few web-interfaces that you may want to browse: http://localhost:50070HDFS webui

http://localhost:50030 MapReduce webui

http://localhost:50060 Task Tracker webui

##### Stopping hadoop

```
$HADOOP_HOME/bin/stop-all.sh
```

Sometimes it’s useful to start and stop each Hadoop service separately. Under$HADOOP_HOME/bin you will find a series of scripts dedicated to starting and stopping individual services. Their name will hint the services they start or stop.

### Hbase

#### Downloading Hbase

Now that you have successfully setup and launch Hadoop it’s time to install Hbase. Similarly to Hadoop, you have two options to get Hbase. You can either go to the Hbase distribution site, choose a mirror close to your location and download it (then copy to $HD_HOME), or execute the following commands:

```
cd ~/Downloads
curl http://apache.websitebeheerjd.nl/hbase/stable/hbase-0.94.8.tar.gz > hbase-0.94.8.tar.gz
mv hbase-0.94.8.tar.gz $HD_HOME/
cd $HD_HOME
tar xvzf hbase-0.94.8.tar.gz
ln -s hbase-0.94.8 hbase
```

#### Configuring Hbase

Configuring Hbase is quite easy (a very basic instance), you need to modify only two files (located under ``$HBASE_HOME/conf``).

##### hbase-env.sh

The file ``hbase-env.sh`` sets the execution environment for Hbase. This file works the same way with as hadoop-env.sh for Hadoop. Add the following lines to ``hbase-env.sh``:

```
JAVA_HOME=/System/Library/Frameworks/JavaVM.framework/Versions/CurrentJDK/Home
HBASE_OPTS="-Djava.net.preferIPv4Stack=true -Djava.security.krb5.realm=OX.AC.UK -Djava.security.krb5.kdc=kdc0.ox.ac.uk:kdc1.ox.ac.uk" 
```

##### hbase-site.xml

Hbase properties are governed by the file hbase-site.xml. The only configuration parameter that you need to specify to make Hbase work is hbase.rootdir, the Hbase root directory. This directory can be either a local file file:/// or an HDFS instancehdfs://. In this particular case we are pointing Hbase to our newly installed HDFS instance. Other properties that can be set in this files can be found here.

Hbase requires Zookeper to work. By default Hbase comes with an embedded instance of Zookeeper, which relieves us from the task of setting one by ourselves. In the case that you may want to know more about Zookeper, its configuration, and its role on the Hbase architecture checkout this article.

```xml
<configuration>
    <property>
    <name>hbase.rootdir</name>
    <value>hdfs://localhost:9000/hbase</value>
    </property>
</configuration>
```

#### Running Hbase

Now you are ready to launch with Hbase. To start Hbase just execute the following command:

```
$HBASE_HOME/bin/start-hbase.sh
```

##### Test it

In order to test your Hbase installation, launch the Hbase shell and play with it (heavily inspired from http://hbase.apache.org/book/quickstart.html). To launch the Hbase shell execute the following command:

```
$HBASE_HOME/bin/hbase shell
```

You should be prompted to the Hbase interactive interpreter:

```
HBase Shell; enter 'help' for list of supported commands.
Type "exit" to leave the HBase Shell
Version 0.94.8, r1485407, Wed May 22 20:53:13 UTC 2013
```

Create a new table and put new values on it:

```
hbase(main):003:0> create 'test', 'cf'
0 row(s) in 1.2200 seconds
hbase(main):003:0> list 'test'
..
1 row(s) in 0.0550 seconds
hbase(main):004:0> put 'test', 'row1', 'cf:a', 'value1'
0 row(s) in 0.0560 seconds
hbase(main):005:0> put 'test', 'row2', 'cf:b', 'value2'
0 row(s) in 0.0370 seconds
hbase(main):006:0> put 'test', 'row3', 'cf:c', 'value3'
0 row(s) in 0.0450 seconds
```

scan the table values:

```
hbase(main):007:0> scan 'test'
ROW        COLUMN+CELL
row1       column=cf:a, timestamp=1288380727188, value=value1
row2       column=cf:b, timestamp=1288380738440, value=value2
row3       column=cf:c, timestamp=1288380747365, value=value3
3 row(s) in 0.0590 seconds
```

get a value through its key:

```
hbase(main):008:0> get 'test', 'row1'
COLUMN      CELL
cf:a        timestamp=1288380727188, value=value1
1 row(s) in 0.0400 seconds
```

disable and drop (delete) the table.

```
hbase(main):012:0> disable 'test'
0 row(s) in 1.0930 seconds
hbase(main):013:0> drop 'test'
0 row(s) in 0.0770 seconds 
```

If you could execute those commands successfully then your hbase instance is working properly.

#### Hbase web-interfaces

http://localhost:60010/ Hbase master webui

http://localhost:60030/ Hbase region server webui

#### Stopping Hbase

```
$HBASE_HOME/bin/stop-hbase.sh
```