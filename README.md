#Hadoop HDFS 配置1234

##服务器
|IP|Domain|Name Node|Journal Node|ZKFC|Data Node|zookeeper|
|:--|:--:|:--:|--:|--:|--:|--:|
|192.168.10.100|hadoop-100|nn1(hdfs-ha1)| |Y| |Y|
|192.168.10.101|hadoop-101|             |Y| |Y| |
|192.168.10.102|hadoop-102|nn3(hdfs-ha2)|Y|Y|Y| |
|192.168.10.103|hadoop-103|nn2(hdfs-ha1)| |Y| |Y|
|192.168.10.104|hadoop-104|nn4(hdfs-ha2)|Y| |Y|Y|

