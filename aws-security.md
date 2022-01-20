## security basic
* CIA 
  confidentiality -> IAM, MFA
  integrity (data integrity and authorization to change it) -> certificate management, bucket policy  
  availability -> auto scaling, multi-AZ, fail-over
* AAA
  authentication -> IAM
  authorization -> Policies
  accounting -> CloudTrail
* Non-repudiation (the Triad) can't deny you did something

## security of AWS
https://aws.amazon.com/security/
complicance
https://aws.amazon.com/compliance/

aws corp network is segregated from AWS 
PCI, ISO, HIPAA

## shared responsibility model
model chnages for different types:
* infrastructure: ec2, auto scaling, vpc, you control the os, you configure and operate any id management
* container; RDS, EMR, Elastic Beanstalk ;managing network control, like firewall rules
* abstracted; S3, Glacier, SQS; abstract service policy

## security in AWS
* visibility
  - AWS Config - get a clear picture of the assets; it has rules like check acm certificate expiration, etc
* Auditablity
  - AWS CloudTrail
* Controllability
  - AWS KMS(multi tenant), AWS CloudHSM (dedicated, FIPS 140-2 COMPLIANCE)
* Agility
  Adapt to change:
  - Cloudformation
  - Elastic Beanstalk
  Process repeatable:
  - AWS Opsworks
  - AWS CodeDeploy
* Automation
* Scale

## Recap IAM