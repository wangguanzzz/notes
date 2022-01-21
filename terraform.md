## download old version path
https://releases.hashicorp.com/terraform/

## local plugin installation path
e.g aws plugin

/home/ansuser/.terraform.d/plugins/local/hashicorp/aws/3.36.0/linux_amd64

download path

https://releases.hashicorp.com/terraform-provider-aws/

if terraform updated, need to remove the local lock file **.terraform.lock.hcl**

## import resource
```console
terraform import  aws_kms_key.cmk arn:aws:kms:ap-east-1:379749426479:key/2d4b909c-4f9e-46c7-b2aa-52adf27d3565
# import into module
terraform import  module.cmk.aws_kms_key.cmk arn:aws:kms:ap-east-1:379749426479:key/2d4b909c-4f9e-46c7-b2aa-52adf27d3565
```
## statement operation
only remove in tf state
```console
terraform state rm 'module.foo.packet_device.worker'
```

## retrieve root variable from module
from root
```
module "central_logging" {
  source = "./central-logging"
  account_id    = var.account_id
}
```
in module, the same variable need to be delared with empty
```
#variables.tg
variable "account_id" {
}

```
