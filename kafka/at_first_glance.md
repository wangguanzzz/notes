## application metrics
- producer
- consumer
- stream processor ( producing messages into output streams)
- connector (eg. 接入数据库into kafka)

## messages and schemas
3 ways of message delivery from producer to consumer
1. at least once (default)
2. at most once 
3. exactly once 

## topic and partitions
stream of messages => topic
multiple partitions make one topic, each partition has accumulation messages starting from 0  to inf
each message in partition has a unique id, that is offset. offset expired by one week by default

## brokers and clusters
brokers are in charge of electing a leader and replicating data across the broker service
a cluster consists of a set of broakers