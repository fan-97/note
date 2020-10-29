{
	"bootstrap.servers": "11.33.144.54:9092",
	"compression.type": "none",
	"max.request.size": 10485760,
	"acks": "0",
	"retries": 0,
	"batch.size": 16384,
	"linger.ms": 1,
	"buffer.memory": 33554432,
	"key.serializer": "org.apache.kafka.common.serialization.StringSerializer",
	"value.serializer ": "org.apache.kafka.common.serialization.StringSerializer",
	"sasl.jaas.config": "org.apache.kafka.common.security.plain.PlainLoginModule required \n username=\"这里填账号\" \n password=\"这里填密码\";",
	"sasl.mechanism": "PLAIN",
	"security.protocol": "SASL_PLAINTEXT"
}
