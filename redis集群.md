1.集群中各实例redis配置文件，注单修改端号号
port 7000
cluster-enabled yes
cluster-config-file nodes.7000.conf
cluster-node-timeout 5000
appendonly yes
daemonize yes

2.创建配置文件夹
mkdir redis-cluster
cd redis-cluster
mkdir 7000 7001 7002 7003 7004 7005

3.将过程1中的配置文件分别复制到过程2中对应的文件夹中并修改端口号和cluster-config-file

4.使用redis-server /path/redis.conf命令分别启动六个实例

5.使和如下命令创建三主三从redis集群
redis-cli --cluster create 127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005 --cluster-replicas 1


参考  ：https://www.jianshu.com/p/a1e62e78667c
