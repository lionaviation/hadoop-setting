# Hadoop HDFS 配置1234

## 服务器部署

|IP|Domain|Name Node|Journal Node|ZKFC|Data Node|zookeeper|
|:--|:--:|:--:|--:|--:|--:|--:|
|192.168.10.100|hadoop-100|nn1(hdfs-ha1)| |Y| |Y|
|192.168.10.101|hadoop-101|             |Y| |Y| |
|192.168.10.102|hadoop-102|nn3(hdfs-ha2)|Y|Y|Y| |
|192.168.10.103|hadoop-103|nn2(hdfs-ha1)| |Y| |Y|
|192.168.10.104|hadoop-104|nn4(hdfs-ha2)|Y| |Y|Y|

## 启动过程
### 1、启动Journal Node

    hadoopp-daemon.sh start journalnode
    
### 2、格式化HDFS
在nn1和nn3执行

    hdfs namenode -format
    hadooop-daemon.sh start namenode
    
在nn2和nn4执行

    hdfs namenode -bootstrapStandby
    
在nn1和nn3执行

    hadoop-daemon.sh stop namenode
    
### 3、格式化ZKFS

    hdfs zkfc -formatZK
    
### 4、启动HDFS
在nn1执行

    start-dfs.sh
