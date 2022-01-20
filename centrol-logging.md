
## cloudwatch agent
1. collect file can add * collect more file
2. docker 
--log-driver=awslogs => send logs to cloudwatch, log groups add to -options

## ES
source -> lambda
* deploy_es -> lambda used to config ES;
 data.ini, configuration data; 
  1. conponent template
  2. index template -> 通配符 创建新的index
* es_loader -> 
