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
1. when get MFA QR code picture, better to take a photo and save it in s3. in case the mobile is lost. 
2. don't give root user key/secret key access
3. power user doesn't have access to IAM

## S3 bucket policy
typical usage:
* grant cross account access to bucket
* your bucket policy is greater than 10k and smaller than 20k

the bucket policy needs the target , not only the bucket arn , it needs to be arn:xxx:s3//*
* bucket policy apply to the whole bucket
* ACL apply to the objects

the rule of the thumb
**The explicit deny always trumps, the deny over allow**
**least privilege in aws, if nothing is said, by default is deny**
## S3 ACL
aws encourage to use IAM and bucket policy, below is the use case for ACL
1. need fine grained permissions on individual files/objects
2. if the policy is larger than 20K

permission control on a specific AWS user can be only done with API/cli , NOT in the console

