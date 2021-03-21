---
title: hadoop在Linux平台安装
date: 2019-09-25 21:50:35
tags:
- Hadoop
- Linux
---
### 1. 解压好放在/usr/local位置
  * 配置环境变量
编辑/etc/profile文件
<!--more-->
```shell
export HADOOP_HOME=/usr/local/hadoop-2.7.6
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
```
   * 启用source /etc/profile来启动环境变量配置


### 2. 进入/usr/local/hadoop创建tmp文件夹
* 创建文件夹  
 $HADOOP_HOME$/tmp/dfs/data   
  $HADOOP_HOME$/tmp/dfs/name
* 编辑/etc/hadoop/core-site.xml
```xml
<configuration>
        <property>
             <name>hadoop.tmp.dir</name>
             <value>file:/usr/local/hadoop/tmp</value>
             <description>Abase for other temporary directories.</description>
        </property>
        <property>
             <name>fs.defaultFS</name>
             <value>hdfs://localhost:9000</value>
        </property>
</configuration>
```
* 编辑/etc/hadoop/hdfs-site.xml
```xml
<configuration>
        <property>
             <name>dfs.replication</name>
             <value>1</value>
        </property>
        <property>
             <name>dfs.namenode.name.dir</name>
             <value>file:/usr/local/hadoop/tmp/dfs/name</value>
        </property>
        <property>
             <name>dfs.datanode.data.dir</name>
             <value>file:/usr/local/hadoop/tmp/dfs/data</value>
        </property>
</configuration>
```
### 启动命令
* `./bin/hdfs namenode -format`
* `./sbin/start-dfs.sh`
### 查看启动是否成功
* `jps`  
可以看到有
14304 Jps  
13431 NameNode  
13863 SecondaryNameNode  
13627 DataNode  
* 浏览器中输入localhost:50070查看控制信息
