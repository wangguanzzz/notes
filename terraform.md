## download old version path
https://releases.hashicorp.com/terraform/

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
