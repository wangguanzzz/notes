## brokers
every broker has its id, this is important to keep it into zookeeper.
if the broker lose connection, with the id and data replication , we can bring it back.'

## replica
leader replica (it is actually the partition itself)
- to make other replicas in-sync, if replica > 10 sec cannot in-sync, it will be marked as out-of-sync replica
follower replica 

don't put two replica on same rack (physical server)