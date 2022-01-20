## download old version path
https://releases.hashicorp.com/terraform/

## local plugin installation path
e.g aws plugin
/home/ansuser/.terraform.d/plugins/local/hashicorp/aws/3.36.0/linux_amd64

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
