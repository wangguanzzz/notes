
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
  3. index-rollover -> choose index retention policy
* es_loader ->  
 1. aws.ini: configuration; ecs: common schema;
 2. user.ini: configuration, not aws part (app-log, terrform log, time get from the file name)
    script_ecs, use script further
    add account info in this part siem=> __init__.py => __call__ method => sf_xx transform, enrich

## Dashboard
saved objects => export the board to json, then import to prod env
source -> saved objects, default dashboard
source -> saved object -> additional , customzing dashboard  siem => sf_  ECS (elastic common schema) cloud.account.id cloud.account.name
**transform_to_ecs** method, according to account id fill accoutn.name fields
