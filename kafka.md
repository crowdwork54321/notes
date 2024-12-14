#创建一个叫topic2的话题，有两个分区，每个分区3个副本
./kafka-topics.sh --zookeeper zookeeper:2181/kafka --create --topic topic3 --replication-factor 1 --partitions 2


#删除一个话题
./kafka-topics.sh --zookeeper zookeeper:2181/kafka --delete  --topic test


#查看一个话题详情
./kafka-topics.sh --zookeeper zookeeper:2181/kafka --describe  --topic topic2


#查看有哪些topic
./kafka-topics.sh --list --zookeeper zookeeper:2181/kafka


#查看某个topic对应的消息数量
./kafka-run-class.sh  kafka.tools.GetOffsetShell --broker-list localhost:9092 --topic user_amount --time -1


#显示所有消费者
./kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list

#查看topic消息内容
./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic lotteryQueue --from-beginning


#查看kafka消费组(详情参考:https://www.cnblogs.com/xiao987334176/p/10199994.html)
./kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list  

#查看消费组的消费情况
./kafka-consumer-groups.sh --describe --bootstrap-server localhost:9092 --group usercenter





