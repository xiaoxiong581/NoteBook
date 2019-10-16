### kafka
##### 创建topic
`./kafka-topics.sh --zookeeper ip:2181 --create --topic test_topic --partitions 30  --replication-factor 2`
##### 查询topic列表
`./kafka-topics.sh --zookeeper ip:2181 --list`
##### 查询指定topic信息
`./kafka-topics.sh --zookeeper ip:2181 --describe --topic test_topic`
##### 生产数据
`./kafka-console-producer.sh --broker-list ip:9092 --topic test_topic`
##### 消费数据
`./kafka-console-consumer.sh  --zookeeper ip:2181  --topic test_topic --from-beginning`
##### 查看消费者group列表
`./kafka-consumer-groups.sh --bootstrap-server ip:9092 --list`
##### 查看消费者topic消费情况
`./kafka-consumer-groups.sh --bootstrap-server ip:9092 --group group_id --describe`
