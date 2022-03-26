## tools to build a resilient systems
* timeout -- avoid requests hold on too many resources ( memory, threads, connections, ephemeral ports)
* retries -- allow clients to survive the partial failures and transient failure
* backoff -- increases the time between subsequent retries


## design of API
idempotent -- means it can be safely retried ( like the payment request, can be sent out multiple times, but the money only move one time)

## classic design pattern
1. singleton
2. facade ( the ront of the building): hide the internal details
3. bridge ( adapter interface)
4. strategy pattern (fundattion + filter) ( customer contact engin -> 1. customer selection strategy 2. notification strategy)
5. observer pattern: publisher -> subscriber  (message queue), loosely coupled    crons -> difficult to debug
message bus

## design pattern for distributed system
* 3 layer design (mvc )
presentation -> business -> database
reacts clients -> nginx/ELB -> nodes server -> cassandra (hashing distributed storaged with replica)
scalability of the 3 layer

*  sharded 
requests -> sharing routes -> isolated applicatoin server and storage
client isolatoin is easy, 
known, simple technology

disadvatnage
1. complexity of router ,monitor and logging
2. no comprehensive view of data

* Lambda
streaming ( unbounded , incoming logs) vs batch (bounded, you know the size, like a file, db)
lambda assumes unbounded , immutable data
1. long-term storage, bounded analysis, high latency ( kafa -> cassandra+spark -> cassandra )
2. temporary queuing, unbounded, low latency  (kafka -> event framework -> cassandra)

* Messaging
Producer -> Topic -> Consumer  
kafka and other queue system -> many message producer and consumer in the same time.

## how to build a reliable system
https://www.youtube.com/watch?v=C3FL00zsaRA
