## architecture
* prometheus server - a central server that gathers metrics and makes them available
* exporters - agents that expose data about systems and application for collection by prometheus server

prommetheus Pull model
server always pull data from exporters

special component
- pushgateway,  eg. for short live batch job
- alert manager

## use cases
limitation
* 100% accuracy
* non time-series data  (eg. log aggregation)  Prom is built to monitoring time-series (numberic) data

## Metrics and Labels
在Prom里面 metric比如CPU只是一个key, 还有具体的metric labels,它一般在{} 里面，比如可以包含cpu 号， server , cpu idle, io, etc.
## metric types
- counter 只能是0,或者慢慢增加的数字, can only be reset
- gauge 一个可以增加和减少的数字
- histogram 指在一个区间(bucket)范围内的， 事件发生的次数 e.g  http_request_duration_seconds_bucket{le="0.3"} ,一般这类metric 有sum and count, eg. http_request_duration_seconds_sum and http_request_duration_seconds_count 
- summary (rarely used) 和histrogram类似， 但是使用的不是离散值，而是pertentile, quantile, 例如： http_request_duration_seconds{quantile="0.95"} 5%最长request的数量