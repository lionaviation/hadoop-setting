# Hadoop HDFS(ViewFS) 配置

## 服务器部署

|IP|Domain|Name Node|Journal Node|ZKFC|Data Node|zookeeper|ResourceNode|HistoryServer|
|:--|:--:|:--:|--:|--:|--:|--:|--:|--:|
|192.168.10.100|hadoop-100|nn1(hdfs-ha1)| |Y| |Y| | |
|192.168.10.101|hadoop-101|             |Y| |Y| | | |
|192.168.10.102|hadoop-102|nn3(hdfs-ha2)|Y|Y|Y| | | |
|192.168.10.103|hadoop-103|nn2(hdfs-ha1)| |Y| |Y|Y|Y|
|192.168.10.104|hadoop-104|nn4(hdfs-ha2)|Y| |Y|Y|Y| |

## 前置条件
### 1、安装hadoop，并配置$HADOOP_HOME和$HADOOP_CONF_DIR
### 2、安装zookeeper，并配置$ZK_HOME
### 3、将本工程所有文件复制到每台机器的$HADOOP_HOME/etc/hadoop/中（可能需要覆盖原有文件）

## 启动过程
### 1、同步每台服务器的秘钥，以上五台服务器互相可以免密码ssh登录

### 2、在每台服务器建立目录
    创建$HADOOP_HOME/data/hadoop_tmp_dir和$HADOOOP_HOME/data/journal_node目录

### 3、启动Journal Node

    hadoopp-daemon.sh start journalnode
    
### 4、格式化HDFS
在nn1执行

    hdfs namenode -format
    hadoop-daemon.sh start namenode
    
查看ClusterID,在nn1执行
    
    cat $HADOOP_HOME/data/hadoop_tmp_dir/dfs/name/current/VERSION

cat $HADOOP_HOME/data/dfs/name/current/VERSION

    hdfs namenode -format -clusterid <Cluster ID>
    hadoop-daemon.sh start namenode

在nn2和nn4执行

    hdfs namenode -bootstrapStandby
    
在nn1和nn3执行

    hadoop-daemon.sh stop namenode
    
### 5、格式化ZKFS
在nn1,nn2,nn3和nn4执行

    hdfs zkfc -formatZK
    
### 6、启动zkfc
在nn1,nn2,nn3和nn4启动zkfc

    hadoop-daemon.sh start zkfc
    
### 7、启动HDFS
在nn1执行

    start-dfs.sh
    
### 8、为每个命名空间建目录
在任意一台一台服务器执行

    hdfs dfs -fs hdfs://hadoop-100:9000 -mkdir /tmp
    hdfs dfs -fs hdfs://hadoop-102:9000 -mkdir /data
    
### 9、启动yarn
在任意一台服务器执行

    start-yarn.sh

### 10、启动History Server
在hadoop-103执行

    mr-jobhistory-daemon.sh start historyserver  
    
注：这里假设hadoop-100和hadoop-102是active状态。
