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
在nn1执行

    hdfs namenode -format
    hadoop-daemon.sh start namenode
    
在nn3执行(cluster id在nn1格式化后生成，可以在web界面查看"http://hadoop-100:50070")

    hdfs namenode -format -clusterid <Cluster ID>
    hadoop-daemon.sh start namenode

在nn2和nn4执行

    hdfs namenode -bootstrapStandby
    
在nn1和nn3执行

    hadoop-daemon.sh stop namenode
    
### 3、格式化ZKFS
在nn1,nn2,nn3和nn4执行

    hdfs zkfc -formatZK
    
### 4、启动zkfc
在nn1,nn2,nn3和nn4启动zkfc

    hadoop-daemon.sh start zkfc
    
### 5、启动HDFS
在nn1执行

    start-dfs.sh
    
### 6、为每个命名空间建目录
在任意一台一台服务器执行

    hdfs dfs -fs hdfs://hadoop-100:9000 -mkdir /tmp
    hdfs dfs -fs hdfs://hadoop-102:9000 -mkdir /data2
    
注：这里假设hadoop-100和hadoop-102是active状态。
