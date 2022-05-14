# introduction
primary job of architect
- performance
- scalability
- reliability
- security 
- deployment
- technology stock

# System performance
## what is  performance
measure how fast of a system under
- a igven workload ( backend data, requrest volume)
- a given hardware ( kind, capacity)

every performance problem is the result of some queue builiding somewhere network socket queue, DB IO queue, OS run queue, etc

two types of request processing
- serial 
- concurrent 

## system performance objects
- minize request-reposonse latency (measure by time unit,  wait/idle time + processing time)
- maximize throughput ( measures by rate of reqeust processing  latency + capacity)

## performance measurement metrics
* latency   (affect user experience)
* throughput ( affecut number of users that can be supported)
* errors ( functional correctness)
* resource saturation ( hareware capacity required)
* tail latency ( indication of queuing of requests get worse with higher workloads) （所有latency 中最慢的1%） because average latency hides the effects of tail latency

## Serial request latency
the SSL/TLS connection is quite costly because of hand shaking

## Minimizing network transfer latency
- connections
* connnection pool
* browser leve persistent connections ( browser support )
- data transfer
* session/data caching
* static data caching
* data format & compression ( look for proper protocal, binary protocals, like binary REST ) ( normally the transfer benefit > cpu cycles ) 
* SSL session caching ( server ccan cache parameters that client transfer to server)
