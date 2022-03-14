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
