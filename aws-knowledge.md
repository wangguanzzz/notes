## cloudwatch collect on-prem server metrics
1. make the below endpoint accessible from on-prem server
```
xxx  s3.ap-east-1.maazonaws.com
xxx  monitoring.ap-east-1.maazonaws.com
xxx  logs.ap-east-1.maazonaws.com
xxx events.ap-east-1.maazonaws.com
```
2. use #amazon-cloudwatch-agent-config-wizard, it will help you create one json in /opt/aws/amazon-cloudwatch-aging/bin
3. add your cloudwatch credential, in your ~/.aws/credentials , add credentials for cloudwatch agent
```
[AmazonCloudWatchAgent]
aws_access_key_id = 
aws_secret_access_key =
region =  
```
4. use command start 
```
amazon-cloudwatch-agent-ctl -a fetch-config -m onPremise -s -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json
```
## cloud migration strategies
1. re-host "lift and shift"
2. re-platform "lift and reshape"  mysql -> RDS
3. re-purchase        local JIRA -> JIRA cloud version
4. rearchitecture  redesign in a cloud-native manner
5. retain, keep on prem

TOGAF
* a standard for enterprice architecture, adopted in many fortume 500 companies
* ofen fills a vacuum
* people take it too literally
* TOGAF is not a cookbook

cloud adoption framework
* Business - strong business case,  measure benefit (ROI)
* plafform 
* people - organization roles, training
* security - cloud security
* governance - determining cloud eligibility,  portfolio management
* operations

## ADOT collect JVM parameters
1. JMX exporter open 9404 port from the pod
2. OpenTelemetry Collector scrape the metrics
3. push the metrics to cloudWatch 

## JAVA tricks
* turn on java GC log -Xlog:gc=debug:file=/tmp/xxx:time,uptime,level,tags:filecount=5,filesize=100m
* jacoco, can be turn on in Maven