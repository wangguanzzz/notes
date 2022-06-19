## overview
in prod, zookeeper should be install on sperate server than kafa

## install kafa and zookeeper with docker
trick : **add user to docker group**
```
sudo usermod -a -G docker <username>
```
kafka need to install with zookeeper

## breaking down the commands
examples
```
./bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka --topic test1 --create --partitions 3  --replication-factor 3
```
it can be multiple zookeeper instances
```
./bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka,zookeeper2:2181/kafka --topic test1 --create --partitions 3  --replication-factor 3
```

list topics
```
./bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka,zookeeper2:2181/kafka --list

```
describe topic
```
./bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka,zookeeper2:2181/kafka --topic test1 --describe
```

delete topic
```
./bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka,zookeeper2:2181/kafka --topic test1 --delete
```
## publishing messages to topic

```
# acks option enable the kafka send a receive message
./bin/kafka-console-producer.sh --broker-list kafka1:9092 --topic test --producer-property acks=all
> <input message one by one>
```
you can publish message to non-existing topic, that will create a new topic
the topic setting is based on server.properties in kafka

## subscribeing to messages

only receive the messages create after this commnad
```
./bin/kafka-console-consumer.sh --bootstrap-server kafka3:9092 --topic test
```
replay from beginning
```
./bin/kafka-console-consumer.sh --bootstrap-server kafka3:9092 --topic test --from-beginning
```

## multiple comsumers subscribeing to messages
client1
```
./bin/kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic test --group app1
```
client2
```
./bin/kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic test --group app1
```

the producer's messages will be split and received by either one of the clients

describe the group could check the current-offset and lag
```
./bin/kafka-consumer-groups.sh --bootstrap-server kafka:9092 --describe --group app1
```