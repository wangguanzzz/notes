## introduct to terraform
it can also manage: DNS enries, Saas features
terraform file format: .tf  .tfvars

concept
1. infrastrucutre as code
2. execution plans
3. resource graph
4. change automation

# fundamentals

## cli
```
terraform -chdir=<path_to/tf> <subcommand>

#initialize hte directory
terraform init

terraform plan
terraform apply
terraform destroy

# output a deployement plan
terraform plan -out <plan_name>
# output a destroy plan
terraform plan -destroy
# apply a specific plan
terraform apply <plan_name>

```

```
# resource and vairable cli option
terraform apply -target , -var

# get provider info used in configuration
terraform providers

```

## configuration language
file support both .tf and .tf.json
must be utf-8
override tf will overried specific attribute

syntax

identifiers: argument names, block type names, resources, input variable, etc
```
  resource "aws_instance" "example" {
    # argument
    ami = "abc123"

    # block
    network_interface {

    }
  }
```

## working with resources
resource types:
- providers
- arguments
- documentation

meta-arguments:
- depends_on
- count
- for_each
- provider
- lifecycle
- provisioner and connection
some resources has timeouts

how config applied
- create
- destroy
- update in-place
- destroy and re-create

access resource attributes
- expressions with Terraform modules
- read-only attributes with information obtained from remote APIs
- provider-included data sources

local-only resources
- ssh keys
- self-signed certs
- random ids

## Input variables
varialbe block, below is the optional arguments for variable def:
- default
- type
- description
- validation
- sensitive (如果在attribute里面sensitive=true，那么在output里面会显示（sensitive))

type constrains
- string
- number 
- bool

type constructors
- list(<type>)
- set(<type>)
- map(<type>)
- object({<attribute> = <type>, ...})
- tuple([<type>,...])

a value can be accessed from an expression, var.<var_name>, **a value assigned to a variable can only be accessed in an expression within the module it was declared**

how to assign values
- in a terraform cloud workspace
- individually, with the -var command line opiton
- in variable definition files like .tfvars or .tfvars.json
- as environment variables

```
terraform apply -var-file="testing.tfvars"

# environment variable example
export TF_VAR_image_id=ami-abc123
terraform plan
```

the order in which variables are loaded:  (override 3>2>1 )
1. environment variables
2. terraform.tfvars
3. any command-line options like -var and -var-file 

## Declearing output variables
uses
- 子module expose atrribute to parent module
- a root module use htme to print value in CLI
- root module outputs can be accessed by ohter configurations  via the terraform_remote_state data source

```
output "instance_ip_addr" {
  value = aws_instance.server.private_ip
}
```

access the child module output
```
module.<module_name>.<output_name>
```
optional arguments
- description
- sensitive
- depends_on

the depends_on argument should be used as a last resort. You should always include a comment explaining why it is being used

## declaring local variable
use
- assign a name to an expression
- use the variable multiple times within a module without repeating it

```
locals {
  service_name = "forum"
  owner = "chris"
}
```

invoke
```
resource "instance" "example" {
  #...
  tags = local.common_tags
}
```

## modules
container of how resource is built
3 types of modules
- root module
- child modules
- published module

invoke a module
```
module "servers" {
  source = "./path"
  servers = 5
}
```

module arguments
- source
- version ( used when module is from a registory)
- input variables
- meta-arguments like  for_each and depends_on

meta-arguments
- count
- for_each
- providers
- depends_on

child modules can declare output values to selectively export values which are accessible by the calling module

### transferring resource state
when transfer resources into modules,  need to use **terraform state mv** to inform terrafrom
- reources in child module must be  prefixed with module.<module_name>
- if a module is called with count or for_each,  it must be prefixed with module.<module_name>[<index>]

### tainting resources within module
it is not possible to taint entire module. each resource within the module must be tainted separately
```
terraform taint module.salt_master.aws_instance.salt_master
```

## module sources
- local paths
```
source = "./consul"
```
- terraform registry
```
# hashicorp registry
source = "hashicopr/consul/aws"
version = "0.1.0"
# other registry
source = "app.terraform.io/example/k8s-cluster/azurerm"
version = "1.1.0"
```

- github
```
source = "github.com/consul/example"
#ssh
source = "git@github.com:hashicorp/example.git"
```
- bitbucket
- generic git, mercurial repositories
```
# master brach
source = "git::https://example.com/vpc.git"
# branch or tag names
source = "git::https://example.com/vpc.git?ref=v1.0"
```
- HTTP URLs
```
source  "https://example.com/vpc.zip"
```
- S3 buckets
- GCS buckets

## expressions and functions
- expressions are used to reference/ compute values within a configuraiton
- functions are used to transform and combine values within a expression

terraform data types:
1. string ""
2. number 1, 2.3
3. bool
4. list/tuple
5. map/object
6. null

7 types of named values in terraform
1. resources  type.name
2. input variables  var.name
3. local values   local.name
4. child module outputs   module.modulename.outputname
5. data sources data.datatype.name
6. filesystem and workspace info    path.module, path.root, path.cwd, terraform.workspace
7. block-local values count.index, each.key, each.value, self

conditional expression:
```
#
condition? true_val : false_val
# example
var.a != "" ?  var.a : "default-a"
```

### functions
you can use terraform console to test function values
```
$ terraform console
```

## backend configuration
- where state is stored
- where operations are performed

config limitation
- a config can only have on backend block
- a backend block cannot refer to named values

## working with state
- meta data
- performance
- sync (within the team)

当有操作时,terraform会lock住state， 需要时可以用下面命令解锁(务必小心)：
```
$ terraform force-unlock [options] LOCK_ID [DIR]
```

command example
```
# state list
terraform state list
# state show
terraform state show 'module.name.foo.worker'
# state mv
$ terraform state mv packet.foo packet.bar
# state remove
$ terraform state rm 'packet.bar'
# state download
$ terraform state pull
there is a push
```

## managing workspaces
workspaces allow you to use the same working copy of your configurations as well as the same plugin and module caches, while still keeping seperate states for each colleciton of resources you manage
可以用同一个代码，控制多套环境，比如test, prod，等等、default workspace is called default
```
terraform workspace select
terraform workspace list
terraform workspace new
terraform workspace delete
```