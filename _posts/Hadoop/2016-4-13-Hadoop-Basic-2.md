---
layout: post
keywords: Hadoop 入门系列
description: Hadoop 入门系列
title: "Hadoop 入门系列2--Hive2.0.0安装"
categories: Hadoop
tags: Hadoop
---

1、Hadoop介绍

1.1 Hadoop简介

Apache Hadoop软件库是一个框架，允许在集群服务器上使用简单的编程模型对大数据集进行分布式处理。Hadoop被设计成能够从单台服务器扩展到数以千计的服务器，每台服务器都有本地的计算和存储资源。Hadoop的高可用性并不依赖硬件，其代码库自身就能在应用层侦测并处理硬件故障，因此能基于服务器集群提供高可用性的服务。

1.2Hadoop生态系统

经过多年的发展形成了Hadoop1.X生态系统，其结构如下图所示：

![1](/public/img/posts/2016-04-13_hadoop_structure.jpg)

+ HDFS--Hadoop生态圈的基本组成部分是Hadoop分布式文件系统（HDFS）。HDFS是一种数据分布式保存机制，数据被保存在计算机集群上，HDFS为HBase等工具提供了基础。

+ MapReduce--Hadoop的主要执行框架是MapReduce，它是一个分布式、并行处理的编程模型，MapReduce把任务分为map(映射)阶段和reduce(化简)。由于MapReduce工作原理的特性， Hadoop能以并行的方式访问数据，从而实现快速访问数据。

+ Hbase--HBase是一个建立在HDFS之上，面向列的NoSQL数据库，用于快速读/写大量数据。HBase使用Zookeeper进行管理，确保所有组件都正常运行。

+ Zookeeper--用于Hadoop的分布式协调服务。Hadoop的许多组件依赖于Zookeeper，它运行在计算机集群上面，用于管理Hadoop操作。

+ Pig--它是MapReduce编程的复杂性的抽象。Pig平台包括运行环境和用于分析Hadoop数据集的脚本语言(Pig Latin)。其编译器将Pig Latin翻译成MapReduce程序序列。

+ Hive--Hive类似于SQL高级语言，用于运行存储在Hadoop上的查询语句，Hive让不熟悉MapReduce开发人员也能编写数据查询语句，然后这些语句被翻译为Hadoop上面的MapReduce任务。像Pig一样，Hive作为一个抽象层工具，吸引了很多熟悉SQL而不是Java编程的数据分析师。

+ Sqoop是一个连接工具，用于在关系数据库、数据仓库和Hadoop之间转移数据。Sqoop利用数据库技术描述架构，进行数据的导入/导出；利用MapReduce实现并行化运行和容错技术。

+ Flume提供了分布式、可靠、高效的服务，用于收集、汇总大数据，并将单台计算机的大量数据转移到HDFS。它基于一个简单而灵活的架构，并提供了数据流的流。它利用简单的可扩展的数据模型，将企业中多台计算机上的数据转移到Hadoop

2.1软硬件环境说明

所有节点均是CentOS系统，防火墙和SElinux禁用，所有节点上均创建了一个hmy用户，并在系统根目录下创建/app目录，用于存放Hadoop等组件运行包。因为该目录用于安装hadoop等组件程序，用户对hmy必须赋予rwx权限（一般做法是root用户在根目录下创建/app目录，并修改该目录拥有者为hmy(chown –R hmy:hmy /app）。

Hadoop搭建环境：

+ 虚拟机操作系统： CentOS6.6  64位，双核，2G内存

+ JDK：1.7.0_79 64位

+ Hadoop：2.6.0

2.2环境搭建

实验环境的虚拟机配置清单：

+ Vmware 12 workstation
+ CentOS: 6.6 64位
+ JDK: 1.7.0_79 64位

2.2.1配置CentOS本地环境

该部分对服务器的配置需要在服务器本地进行配置，配置完毕后需要重启服务器确认配置是否生效，特别是远程访问服务器需要设置固定IP地址。

2.2.1.1设置IP地址

1.点击System-->Preferences-->Network Connections
  
![1](/public/img/posts/2016-04-13_NetWork.png)

2.修改完毕之后，重启电脑，打开终端。输入：ifconfig 查看ip地址。

![1](/public/img/posts/2016-04-13_NetWork_ifconfig.png)

2.2.1.2设置机器名

使用sudo vi /etc/sysconfig/network 打开配置文件，根据实际情况设置该服务器的机器名，新机器名在重启后生效。

![1](/public/img/posts/2016-04-13_network_machine_name.png)

2.2.1.3设置Host映射文件

1.设置IP地址与机器名的映射，设置信息如下：

	sudo vi /etc/hosts
	加入：192.168.1.8 hadoop

![1](/public/img/posts/2016-04-13_machine_hostsname.png)

2.使用如下命令对网络设置进行重启

	sudo /etc/init.d/network restart

3.使用ping命令验证设置是否成功:win7 cmd -> ping 192.168.1.8

2.2.2设置操作系统环境

2.2.2.1关闭防火墙

在Hadoop安装过程中需要关闭防火墙和SElinux，否则会出现异常

1.使用sudo service iptables status 查看防火墙状态，如下所示表示iptables已经开启

![1](/public/img/posts/2016-04-13_firewall.png)

2.以root用户使用如下命令关闭iptables

	chkconfig iptables off


2.2.2.2关闭SElinux

1.使用sudo getenforce命令查看是否关闭，如果显示Enforcing表示没有关闭

2.修改/etc/selinux/config 文件

将SELINUX=enforcing改为SELINUX=disabled，执行该命令后重启机器

![1](/public/img/posts/2016-04-13_selinux.png)

2.2.2.3 JDK安装及配置

1.下载JDK1.7 64bit安装包

打开JDK1.7 64bit安装包下载链接为：

[http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html "JDK Download")

2.创建/app目录，把该目录的所有者修改为hmy

	sudo mkdir /app
	sudo chown -R hmy:hmy /app

![1](/public/img/posts/2016-04-13_hadoop_path.png)

3.创建/app/lib目录，使用命令如下：

	mkdir /app/lib

![1](/public/img/posts/2016-04-13_hadoop_path.png)

4.把下载的安装包解压并迁移到/app/lib目录下

5.使用sudo vi /etc/profile命令打开配置文件，设置JDK路径：

	export JAVA_HOME=/app/lib/jdk1.7.0_79
	export PATH=$JAVA_HOME/bin:$PATH
	export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

6.编译并验证
	
	[hmy@hadoop ~]$ source /etc/profile
	[hmy@hadoop ~]$ java -version
	java version "1.7.0_79"
	Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
	Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)

2.2.2.4更新OpenSSL

CentOS自带的OpenSSL存在bug，如果不更新OpenSSL在Ambari部署过程会出现无法通过SSH连接节点，使用如下命令进行更新：

	yum update openssl

2.2.2.5SSH无密码验证配置

1.使用sudo vi /etc/ssh/sshd_config打开sshd_config配置文件，开放三个配置，如下所示：

	#RSAAuthentication yes
	#PubkeyAuthentication yes
	#AuthorizedKeysFile	.ssh/authorized_keys
	#AuthorizedKeysCommand none
	#AuthorizedKeysCommandRunAs nobody
	去掉#
	RSAAuthentication yes
	PubkeyAuthentication yes
	AuthorizedKeysFile .ssh/authorized_keys
	#AuthorizedKeysCommand none
	#AuthorizedKeysCommandRunAs nobody

2.配置后重启服务

	sudo service sshd restart

3.使用hmy用户登录使用如下命令生成私钥和公钥

	ssh-keygen -t rsa

4.进入/home/hmy/.ssh目录把公钥命名为authorized_keys，使用命令如下：

	cp id_rsa.pub authorized_keys

5.使用如下设置authorized_keys读写权限

	chmod 400 authorized_keys

6.测试ssh免密码登录是否生效

![1](/public/img/posts/2016-04-13_putty_setting.png)

![1](/public/img/posts/2016-04-13_putty_login.png)

3、部署Hadooop2.6.0

3.1配置Hadoop环境

在Apache网站上提供Hadoop2.X安装包只支持32位操作系统安装，在64位服务器安装会出现3.1的错误异常。网上有人已经编译好了64位的安装包，所以我们只要下载下来就可使用

3.1.1  下载并解压hadoop安装包

解压缩并移动到/app目录下

3.1.2  在Hadoop目录下创建子目录

![1](/public/img/posts/2016-04-13_hadoop_path.png)

在hadoop2.6.0目录下创建tmp、name和data目录

![1](/public/img/posts/2016-04-13_hadoop_creat_tmphdfs.png)

	cd /app/hadoop2.6.0
	mkdir tmp
	mkdir hdfs
	cd hdfs
	mkdir hdfs/name
	mkdir hdfs/data

3.1.3配置hadoop-env.sh

1.打开配置文件hadoop-env.sh

	cd /app/hadoop-2.6.0/etc/hadoop
	sudo vi hadoop-env.sh

2.加入配置内容，设置了hadoop中jdk和hadoop/bin路径
 
	export HADOOP_CONF_DIR=/app/hadoop2.6.0/etc/hadoop
	export JAVA_HOME=/app/lib/jdk1.7.0_79
	export PATH=$PATH:/app/hadoop2.6.0/bin

3.编译配置文件hadoop-env.sh，并确认生效
 
	source hadoop-env.sh
	hadoop version

3.1.4配置yarn-env.sh

打开配置文件yarn-env.sh，设置了hadoop中jdk路径，配置完毕后使用source yarn-env.sh编译该文件

	export JAVA_HOME=/app/lib/jdk1.7.0_79

3.1.5配置core-site.xml

1.使用如下命令打开core-site.xml配置文件

	cd /app/hadoop2.6.0/etc/hadoop
	sudo vi core-site.xml

2.在配置文件中，按照如下内容进行配置

	<configuration>
	  <property>
	    <name>fs.default.name</name>
	    <value>hdfs://hadoop:9000</value>
	  </property>
	  <property>
	    <name>fs.defaultFS</name>
	    <value>hdfs://hadoop:9000</value>
	  </property>
	  <property>
	    <name>io.file.buffer.size</name>
	    <value>131072</value>
	  </property>
	  <property>
	    <name>hadoop.tmp.dir</name>
	    <value>file:/app/hadoop2.6.0/tmp</value>
	    <description>Abase for other temporary directories.</description>
	  </property>
	  <property>
	    <name>hadoop.proxyuser.hduser.hosts</name>
	    <value>*</value>
	  </property>
	  <property>
	    <name>hadoop.proxyuser.hduser.groups</name>
	    <value>*</value>
	  </property>
	</configuration>

3.1.6配置hdfs-site.xml

1.使用如下命令打开hdfs-site.xml配置文件

	cd /app/hadoop2.6.0/etc/hadoop
	sudo vi hdfs-site.xml

2.在配置文件中，按照如下内容进行配置

	<configuration>
	  <property>
	   <name>dfs.namenode.secondary.http-address</name>
	   <value>hadoop:9001</value>
	  </property>
	  <property>
	   <name>dfs.namenode.name.dir</name>
	   <value>file:/app/hadoop2.6.0/hdfs/name</value>
	  </property>
	  <property>
	   <name>dfs.datanode.data.dir</name>
	   <value>file:/app/hadoop2.6.0/hdfs/data</value>
	  </property>
	  <property>
	   <name>dfs.replication</name>
	   <value>1</value>
	  </property>
	  <property>
	   <name>dfs.webhdfs.enabled</name>
	   <value>true</value>
	  </property>
	</configuration>

3.1.7配置mapred-site.xml

1.默认情况下不存在mapred-site.xml文件，可以从模板拷贝一份，并使用如下命令打开mapred-site.xml配置文件

	cd /app/hadoop2.6.0/etc/hadoop
	cp mapred-site.xml.template mapred-site.xml
	sudo vi mapred-site.xml

2.在配置文件中，按照如下内容进行配置

	<configuration>
	  <property>
	    <name>mapreduce.framework.name</name>
	    <value>yarn</value>
	  </property>
	  <property>
	    <name>mapreduce.jobhistory.address</name>
	    <value>hadoop:10020</value>
	  </property>
	  <property>
	    <name>mapreduce.jobhistory.webapp.address</name>
	    <value>hadoop:19888</value>
	  </property>
	</configuration>

3.1.8配置yarn-site.xml

1.使用如下命令打开yarn-site.xml配置文件

	cd /app/hadoop2.6.0/etc/hadoop
	sudo vi yarn-site.xml

2.在配置文件中，按照如下内容进行配置

	<configuration>
	  <property>
	    <name>yarn.nodemanager.aux-services</name>
	    <value>mapreduce_shuffle</value>
	  </property>
	  <property>
	    <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
	    <value>org.apache.hadoop.mapred.ShuffleHandler</value>
	  </property>
	  <property>
	    <name>yarn.resourcemanager.address</name>
	    <value>hadoop:8032</value>
	  </property>
	  <property>
	    <name>yarn.resourcemanager.scheduler.address</name>
	    <value>hadoop:8030</value>
	  </property>
	  <property>
	    <name>yarn.resourcemanager.resource-tracker.address</name>
	    <value>hadoop:8031</value>
	  </property>
	  <property>
	    <name>yarn.resourcemanager.admin.address</name>
	    <value>hadoop:8033</value>
	  </property>
	  <property>
	    <name>yarn.resourcemanager.webapp.address</name>
	    <value>hadoop:8088</value>
	  </property>
	</configuration>

3.1.9配置slaves文件

在slaves配置文件中设置从节点，这里设置为hadoop，与Hadoop1.X区别的是Hadoop2.X不需要设置Master
	cd /app/hadoop2.6.0/etc/hadoop
	vi slaves

	[hmy@hadoop hadoop]$ cat slaves 
	hadoop

3.1.10格式化namenode

	cd /app/hadoop2.6.0/bin
	./hdfs namenode -format

3.2启动Hadoop

3.2.1启动hdfs

	cd /app/hadoop2.6.0/sbin
	./start-dfs.sh

3.2.2验证当前进行

使用jps命令查看运行进程，此时在hadoop上面运行的进程有：namenode、secondarynamenode和datanode三个进行

	[hmy@hadoop sbin]$ jps
	2812 NameNode
	3021 SecondaryNameNode
	3580 Jps
	3464 NodeManager
	3171 ResourceManager

3.2.3启动yarn

如果用start-all.sh那么就不用此步。
	cd /app/hadoop2.6.0/sbin
	./start-yarn.sh

3.2.4验证当前进行

使用jps命令查看运行进程，此时在hadoop上运行的进程除了：namenode、secondarynamenode和datanode，增加了resourcemanager和nodemanager两个进程：

	[hmy@hadoop sbin]$ jps
	2812 NameNode
	3021 SecondaryNameNode
	3580 Jps
	3464 NodeManager
	3171 ResourceManager

这时我们发现少了一个DataNode进程，到$HADOOP_HOME/logs目下，使用cat hadoop-shiyanlou-datanode-5****.log（***表示所在机器名）查看日志文件，可以看到在日志中提示：Invalid directory in dfs.data.dir:Incorrect permission for /app/hadoop-1.1.2/hdfs/data, expected:rwxr-xr-x, while actual: rwxrwxr-x

	sudo chmod 755 /app/hadoop2.6.0/hdfs/data

重新启动hadoop集群，可以看到DataNode进程

3.3测试Hadoop

3.3.1创建测试目录

	cd /app/hadoop2.6.0/bin
	./hadoop fs -mkdir -p /class3/input

3.3.2准备测试数据
	./hadoop fs -copyFromLocal ../etc/hadoop/* /class3/input

3.3.3运行wordcount例子

	cd /app/hadoop2.6.0/bin
	./hadoop jar ../share/hadoop/mapreduce/hadoop-mapreduce-examples-2.2.0.jar wordcount /class3/input /class3/output

3.3.4查看结果

使用如下命令查看运行结果：
	./hadoop fs -ls /class3/output/ 
	./hadoop fs -cat /class3/output/part-r-00000 | less

本篇完结！

参考文献

1.http://www.cnblogs.com/shishanyuan/

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@