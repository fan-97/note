1.查看kafka  topic

```
$KAFKA_HOME/bin/kafka-topics --list --zookeeper localhost:2181
```

2.创建topic

```
$KAFKA_HOME/bin/kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
```

3.使用控制台生产者生产消息

```
$KAFKA_HOME/bin/kafka-producer --broker-list localhost:9092 --topic test
```

4.控制台消费者(默认消费最后一个消费组)

```
$KAFKA_HOME/bin/kafka-consumer --bootstrap-server localhost:9092 --topic test
```

5.查看正在运行的消费组

```
$KAFKA_HOME/bin/kafka-consumer-groups --bootstrap-server localhost:9092 --list --new-consumer
```

6.计算消息堆积情况

```
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group  test_kafka_game_x_g1
```

