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

# Querying
PromQL
## Query Basics
- selector  , metric name with labels
  Label matching operator  
  * = equals
  * !=
  * =~ Regex match
  * !~ does not regex match
- range vector selectors  like ... [5m]
- offset modifier  "[5m] offset 1h" a five-minute period one hour ago   

## Query Operators
- arithmetic binary operators
- matching rules  
  * ignoring - ignore the speicified labels when matching
  * on - use only the speicified labels when matching
- comparision binary  operators  默认filter 掉不符条件的数据  可以用 bool operator , 让结果变成 0,或1
- logical/set binary operators, 例如and, 就是找两个metric, 的label完全一样的
- aggregation operators

functions
- rate , a useful function for tracking the average per-second rate of increse in a time-series value, rate(node_cpu_seconds_total[1h])
- clamp_max（metric, 1000) 小于1000算原来，大于1000 算1000

## HTTP API
```
curl localhost:9090/api/v1/query --data-urlencode "query=..."
```

# Exporters
## applicatoin monitoring
- use an existing exporter
- use prometheus pushgateway for batch processsing and short-lived jobs
- use client libraries to build an exporter into your application

每上一个exporter，系统里需要单独建一个系统用户
misc, create user commnad
```
sudo useradd -M -r -s /bin/false apache_exporter
```

```
# /etc/systemd/system/apache_exporter.service
[Unit]
Description=Prometheus Apache Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=apache_exporter
Group=apache_exporter
Type=simple
ExecStart=/usr/local/bin/apache_exporter

[Install]
WantedBy=multi-user.target
```

## job and instances
- instances: prom automatically adds an instance label to metrics. instance are individual endpoints Prom scrapes
- jobs: a job is a collection of instances, all sharing the same purpose. 可以用a job monitoring an application replicas

screpe meta-metrics: Prom automatically created metrics data about scrape


### docker container monitoring
```
# run cadvisor
docker run -d --restart always --name cadvisor -p 8080:8080 -v "/:/rootfs:ro" -v "/var/run:/var/run:rw" -v "/sys:/sys:ro" -v "/var/lib/docker/:/var/lib/docker:ro" google/cadvisor:latest

# check
curl localhost:8080/metrics

```

# Pushgateway
push gateway serves as a middle-an 
- client push metric data to Pushgateway
- Prometheus server pull metrics from Pushgateway

pushgateway setting
```
- job_name" "pushgateway"
  honor_labels: true
  static_configs:
    - targets: [...]
```

## pushing data to pushgateway
/metrics/job/some_job/instance/some_instance

request body:
some_metric{label="val1"} 42

```
[Unit]
Description=Prometheus Pushgateway
Wants=network-online.target
After=network-online.target

[Service]
User=pushgateway
Group=pushgateway
Type=simple
ExecStart=/usr/local/bin/pushgateway

[Install]
WantedBy=multi-user.target
```

## recording rules
recording rules allow you to pre-compute the values of expressions and queries and save the resuls as their own separate set of time-series data.

specify one or more locations for rule files in prometheus.yml under rule_files

enable rule files in prometheus.yml
```
rule_files:
  - "/etc/prometheus/rules/*.yml"

```

rule file example
```
groups:
- name: limedrop_gateway
  rules:
  - record: limedrop_gateway:cpu_usage
    expr: sum(rate(node_cpu_seconds_total{instance='limedrop-gateway:9100',mode!='idle'}[5m])) * 100 / 2
```

# Alertmanager setup and configuration
alertmanager 是 prometheus server外的一个单独的进程. 
alerts are notifications that are triggered automatically by metrics data

## alert manager install
1. 创建alertmanager用户
2. 下载binary
3. 把需要的binary copy 到 /usr/local/bin
4. chown alertmanager:alertmanager 下载的binary
5. 建一个目录，用于存放配置文件 /etc/alertmanager, 把整个文件夹的权限设给特定用户
6. 创建service 文件

## HA alertmanager
 in start option need --cluster.peer
## Managing alert in alert manager
- route
- group ( consolidate multiple alerts into one)
```
route:
  ...

  routes:
  - receiver: 'web.hook'
    group_by: ['service']
    match_re:
      alertname: 'WebServer.*Down'
```
- inhibition ( the network connectivity alert suppresses all of ohter alerts)
```
inhibit_rules:

  ...

  - source_match_re:
      alertname: 'WebServer.*Down'
    target_match:
      alertname: 'WebBadGateway'
```
- silences ( a simple way to temporarily turn off certain notification)

## federation
federation is the process of one Prom server scraping some or all time-series data from anohter Prometheus server.
- hierarchical federation
- cross-service federation

## Prom security
use a reverse proxy