## 1.配置zookeeper

### 1.添加jass配置文件

/root/kafka_zoo_jaas.conf

```
zookeeper{
	org.apache.kafka.common.security.plain.PlainLoginModule required
	username="kafka"
	password="kafka";
};
```

### 2.启动zookeeper

```shell
export KAFKA_OPTS=" -Djava.security.auth.login.config=/root/kafka_zoo_jaas.conf"
zookeeper-server-start.sh $KAFKA_HOME/config/zookeeper.properties >> zookeeper.log 2>&1 &
```

## 2.配置kafka Server

### 1.添加jaas文件

/root/kafka_server_jaas.conf

```
KafkaServer{
	org.apache.kafka.common.security.plain.PlainLoginModule required
    username="kafka"
    password="kafka"
    user_kafka="kafka"
    user_alice="alice"
};
```

### 2.修改server的配置文件

修改`$KAFKA_HOME/config/server.properties` 

```
listeners=SASL_PLAINTEXT://localhost:9092
security.inter.broker.protocol=SASL_PLAINTEXT
sasl.enabled.mechanisms=PLAIN
sasl.mechaism.inter.broker.protocol=PLAIN
```

### 3.启动Kafka Server

```sh
export KAFKA_OPTS=" -Djava.security.auth.login.config=/root/kafka_server_jaas.conf"
kafka-server-start.sh $KAFKA_HOME/config/server.properties >> kafka.log 2>&1 &
```

## 3.配置client

### 1.添加 consumer和producer的JAAS文件

`/root/kafka_client_jaas.conf` 都用同一个配置文件

```
KafkaClient{
	org.apache.kafka.common.security.plain.PlainLoginModule required
	username="kafka"
	password="kafka";
};
```

### 2.修改consumer和producer配置文件

修改 `consumer.properties`和`producer.properties`

```
security.protocol=SASL_PLAINTEXT
sasl.mechanism=PLAIN
```

### 3.启动控制台消费者和生产者

生产者

```sh
export KAFKA_OPTS=" -Djava.security.auth.login.config=/root/kafka_client_jaas.conf"
kafka-console-producer.sh --broker-list localhost:9092 --topic topicName --producer.config $KAFKA_HOME/config/producer.properties
```



消费者

```sh
export KAFKA_OPTS=" -Djava.security.auth.login.config=/root/kafka_client_jaas.conf"
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic topicName --from-beginning --consumer.config $KAFKA_HOME/config/consumer.properties
```



-----

## 后记

配置了SASL的服务端，运行bin目录下的命令都要加上  认证信息

比如：查看消费者组

1. 在./kafka-consumer-groups.sh 添加命令

   export KAFKA_OPTS=" -Djava.security.auth.login.config=/root/kafka_client_jaas.conf"

```shell

[root@Server bin]# cat ./kafka-consumer-groups.sh 
#!/bin/bash
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
# 
#    http://www.apache.org/licenses/LICENSE-2.0
# 
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
export KAFKA_OPTS=" -Djava.security.auth.login.config=/root/kafka_client_jaas.conf"
exec $(dirname $0)/kafka-run-class.sh kafka.admin.ConsumerGroupCommand "$@
```

2. 执行命令 加上 properties

```sh
./kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list --command-config ../config/consumer.properties
```

* java推送消息时候配置sasl认证，在配置文件里面添加下面的内容

```
{
	"sasl.jaas.config": "org.apache.kafka.common.security.plain.PlainLoginModule required \n username=\"这里填账号\" \n password=\"这里填密码\";",
	"sasl.mechanism": "PLAIN",
	"security.protocol": "SASL_PLAINTEXT"
}
```

