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