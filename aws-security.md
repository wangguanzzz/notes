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

## policy conflict
```
"aws:SecureTransport": false
```
means access from http (not https)

## s3 cross region replication
by default , it is using SSL to do replicate
it is 1 to 1 , version on source needed turn on, a ss role will be created.
if the source bucket owner doesn't own the object, the object own nmst grant the bucket owner READ and READ_ACP permission

SSE-C is not replicated, delete can only replicate the delete mark

## securing S3 using cloudfront

* in cloudfront, you could request or import a certificate with ACM (aws certificate manager)
* cloudfront cannot borrow the certificate from loadbalancer
* cloudfront side , it can lock down the access from S3

## using SSL ceritificate with cloudfront
* defualt cloudfront certificate: *.cloudfront.ent domain name access,
* your domain name, you need a custom certificate
* if you use certificate manager, hte certificate must be in the region of cloudfront
* alternatively , certificate can be stored in IAM using IAM cli

## securing s3 using pre-signed URLs
only using the cli, it will generate the URLS
```
aws s3 presign s3://<URL> --expires-in 300 
```
## Security Token Service (STS)
1. work with Federation (AD)
2. Federation with mobile apps
3. crosss account access

**congnito is an Identity Broker (used to mapping the ID in and ID store to STS )
## cognito
1. user -> user pool <-> (JWT tokens)  Facebook, Google, Amazon
2. use JWT token with Identity Pool, get aws credentials
3. user pool to manage user sign-up and sign-in  directly or via web identity providers

## Glacier Vault Lock
Vaults: containers which store one or more Archives
add compliance control for vault: vault lock policy
* similiar to IAM policy
* configure WORM ( write once ,read many)
* create data retention policies
**lock policy are immutable**

## AWS Organizations
consolidate multiple acccount into one organization account
 * for groups/OU access control
 * SCP control, create permission boundary

## AWS CloudTrail
AWS cloudtrail records AWS API calls, in detail level,(source ip, serivce, caller id, response), delivery every 5 minutes
it doesn't record ssh and RDP.
* after-fact incident investigation
* near-real time intrusion detection
* compliance

## AWS config
assess, audit and evaluate the configuration of the AWS resources. like a security camera.

rules: the desired value of the resources
an **aggregator** is an AWS config resource type collects AWS configuration and compliance from
* multiple account
* multiple region
* organizational accounts

it has AWS defined & customizing rules, remediation action can be assigned with rules.
resources can show all aws resources and its compliance situation.

AWS config requires an IAM role , read only permission to resources, write access to s3, publish to SNS
CloudTrail can be used to monitor Config turn off action

## CloudWatch
* CloudWatch
  standard montiroing : 5min, detail: 1min
* CloudWatch Logs
* CloudWatch Events

## Setup Alert if the root user login
* CloudTrail log can be redirected to CloudWatch.
* create a specific metric filter

## CloudHSM
* dedicated hardware security module to ensure data security


# Security Assesment Methodology
1. IAM 
2. Infrastructure Protection
3. Data Protection
4. Detective Controls
5. Incident Response

Tools set
* Prowler Scan

