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

## memory access latency
* finite heap memory
* GC ( run aggresively)
* large heap memory > machine memory => OS will use disk (swapping)
* Finite buffer on DB side

## minimizing memory access latency
* avoid memory bloat ( program level )
* weak/soft reference ( when GC, it will gc first object with weak/soft reference)
* smaller RAM processes is better than one big RAM process ( GC efficiency will be bad)
* GA algorithm (1. batch process 型 (efficient)， 隔一段时间GC一次，停process， 2. real time type (life process).  正规伴随process GC， small pause time)
* normalization 避免数据在数据库重复,使得同样RAM大小下，数据存放最多, compute over storage （如果数据可以算，就不要存） ( for buffer memory)

## disk latency
* logging
* web content files
* DB disk access 

## minimizing disk access latency
* logging 
  - asynchronous logging    main thread do the processing, and logging thread do the logging, (一个小缺点，如果main thread挂了，最后几个statement可能没有被log thread 记录 )
  - sequential & Batch IO   硬盘/RAM 访问分为sequential IO（fast）, random IO（slow）,   context swtiching CPU计算和读写之间的来回切换，越少越好， 所以几条log最好batch在一起
* web content files
  - web content caching, load balancer/ reverse proxy  把找static 的部分指向特定的CDN， dynamic part to applciation server
  - page cache, zero copy
* DB disk access 
  - query optimization
  - data caching
  - schema optimization
    (denomalizaiton vs nomalizaiton) 通常情况选择normalization节约RAM， 但是如果disk latency太大，可以进行denormalization
    indices
  - higher IOPS, RAID, SSD Disk

## CPU Latency
- inefficient algo
- context switching

## minimizing CPU latency
- inefficient algo
  1. efficient algo
  2. efficient queries
- context switching
  1. batch/Async IO
  2. single threaded model ( nodeJS) 适合high load with a lot of IO
  3. thread pool size (thread 数量 和CPU的比例要合适)
  4. mutli-process in virtual env （在大型机上，避免一个process过度占据CPU， 可以设置多个虚拟环境）