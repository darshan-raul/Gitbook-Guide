# Terraform

* The main purpose of the Terraform language is declaring **resources**. All other language features exist only to make the definition of resources more flexible and convenient.
* A group of resources can be gathered into a **module**, which creates a larger unit of configuration.
* A resource describes a single infrastructure object, while a module might describe a set of objects and the necessary relationships between them in order to create a higher-level system.
* A Terraform configuration consists of a **root module**, where evaluation begins, along with a tree of child modules created when one module calls another.
* A module is a collection of .tf or .tf.json files kept together in a directory. The root module is built from the configuration files in the current working directory when Terraform is run, and this module may reference child modules in other directories, which can in turn reference other modules, etc.
* CLI cheatsheet:

> [https://github.com/scraly/terraform-cheat-sheet/blob/master/terraform-cheat-sheet.pdf](https://github.com/scraly/terraform-cheat-sheet/blob/master/terraform-cheat-sheet.pdf)

#### Blocks

are containers for other content and usually represent the configuration of some kind of object, like a resource. Blocks have a block type, can have zero or more labels, and have a body that contains any number of arguments and nested blocks. Most of Terraform's features are controlled by top-level blocks in a configuration file.

#### Arguments

assign a value to a name. They appear within blocks.

#### Expressions

represent a value, either literally or by referencing and combining other values. They appear as values for arguments, or within other expressions.

### RESOURCE SYNTAX:

```text
resource "aws_instance" "web" {
  ami           = "ami-a1b2c3d4"
  instance_type = "t2.micro"
}
```

A resource block declares a resource of a given type \("aws\_instance"\) with a given local name \("web"\).

> The name is used to refer to this resource from elsewhere in the same Terraform module, but has no significance outside of the scope of a module.

> 👍 Resource names must start with a letter or underscore, and may contain only letters, digits, underscores, and dashes.

#### Meta-Arguments

Terraform CLI defines the following meta-arguments, which can be used with any resource type to change the behavior of resources:

* **depends\_on**, for specifying hidden dependencies
* **count**, for creating multiple resource instances
* **provider**, for selecting a non-default provider configuration
* **lifecycle**, for lifecycle customizations
* **provisioner and connection**, for taking extra actions after resource creation

#### Operation Timeouts

Some resource types provide a special timeouts nested block argument that allows you to customize how long certain operations are allowed to take before being considered to have failed.

Each of these arguments takes a string representation of a duration, such as "60m" for 60 minutes, "10s" for ten seconds, or "2h" for two hours.

```text
resource "aws_db_instance" "example" {
  # ...

  timeouts {
    create = "60m"
    delete = "2h"
  }
}
```

### Input Variables

Input variables serve as parameters for a Terraform module, allowing aspects of the module to be customized without altering the module's own source code, and allowing modules to be shared between different configurations.

```text
variable "image_id" {
  type = string
  description = "The id of the machine image (AMI) to use for the server."
}

variable "availability_zone_names" {
  type    = list(string)
  default = ["us-west-1a"]
}
```

> The label after the variable keyword is a name for the variable, which must be unique among all variables in the same module.

#### Assigning Values to Root Module Variables

* **Variables on the Command Line**

To specify individual modules on the command line, use the -var option when running the terraform plan and terraform apply commands:

```text
terraform apply -var="image_id=ami-abc123"
terraform apply -var='image_id_list=["ami-abc123","ami-def456"]'
terraform apply -var='image_id_map={"us-east-1":"ami-abc123","us-east-2":"ami-def456"}'
```

#### Variable Definitions \(.tfvars\) Files

To set lots of variables, it is more convenient to specify their values in a variable definitions file \(with a filename ending in either .tfvars or .tfvars.json\) and then specify that file on the command line with -var-file:

> terraform apply -var-file="testing.tfvars"

A variable definitions file uses the same basic syntax as Terraform language files, but consists only of variable name assignments:

```text
image_id = "ami-abc123"
availability_zone_names = [
  "us-east-1a",
  "us-west-1c",
]
```

**Terraform loads variables in the following order, with later sources taking precedence over earlier ones:**

* Environment variables
* The terraform.tfvars file, if present.
* The terraform.tfvars.json file, if present.
* Any \*.auto.tfvars or \*.auto.tfvars.json files, processed in lexical order of their filenames.
* Any -var and -var-file options on the command line, in the order they are provided. \(This includes variables set by a Terraform Enterprise workspace.\)

### Output Values

Output values are like the return values of a Terraform module

```text
output "instance_ip_addr" {
  value       = aws_instance.server.private_ip
  description = "The private IP address of the main server instance."
  
  depends_on = [
    # Security group rule must be created before this IP address could
    # actually be used, otherwise the services will be unreachable.
    aws_security_group_rule.local_access,
  ]
}
```

### Local Values

A local value assigns a name to an expression, allowing it to be used multiple times within a module without repeating it

```text
locals {
  service_name = "forum"
  owner        = "Community Team"
}
```

> Local values can be helpful to avoid repeating the same values or expressions multiple times in a configuration, but if overused they can also make a configuration hard to read by future maintainers by hiding the actual values used.

## Modules

A module is a **container** for multiple resources that are used together

* Every Terraform configuration has **at least one module, known as its root module**, which consists of the resources defined in the .tf files in the main working directory.
* A module can call other modules, which lets you include the child module's resources into the configuration in a concise way. Modules can also be called multiple times, either within the same configuration or in separate configurations, allowing resource configurations to be packaged and re-used.

// Not quite having handson with modules yet. Will add more once I get to know how to really use it

A complete example of a module following the standard structure is shown below. This example includes all optional elements and is therefore the most complex a module can become:

```text
$ tree complete-module/
.
├── README.md
├── main.tf
├── variables.tf
├── outputs.tf
├── ...
├── modules/
│   ├── nestedA/
│   │   ├── README.md
│   │   ├── variables.tf
│   │   ├── main.tf
│   │   ├── outputs.tf
│   ├── nestedB/
│   ├── .../
├── examples/
│   ├── exampleA/
│   │   ├── main.tf
│   ├── exampleB/
│   ├── .../
```

#### Issues that can be faced

> [https://github.com/hashicorp/terraform/issues/9712](https://github.com/hashicorp/terraform/issues/9712)

## References:

> Ofcourse official documentation : [https://www.terraform.io/docs/configuration/index.html](https://www.terraform.io/docs/configuration/index.html)

* [https://github.com/hashicorp/terraform-guides/tree/master/infrastructure-as-code](https://github.com/hashicorp/terraform-guides/tree/master/infrastructure-as-code)
* [https://github.com/terraform-providers/terraform-provider-aws](https://github.com/terraform-providers/terraform-provider-aws)
* [https://medium.com/@mitesh\_shamra/infrastructure-as-a-code-with-terraform-e7021bf28d7d](https://medium.com/@mitesh_shamra/infrastructure-as-a-code-with-terraform-e7021bf28d7d)
* [https://medium.com/@mitesh\_shamra/state-management-with-terraform-9f13497e54cf](https://medium.com/@mitesh_shamra/state-management-with-terraform-9f13497e54cf)
* [https://medium.com/@mitesh\_shamra/manage-aws-vpc-with-terraform-d477d0b5c9c5](https://medium.com/@mitesh_shamra/manage-aws-vpc-with-terraform-d477d0b5c9c5)
* [https://blog.gruntwork.io/how-to-create-reusable-infrastructure-with-terraform-modules-25526d65f73d](https://blog.gruntwork.io/how-to-create-reusable-infrastructure-with-terraform-modules-25526d65f73d)
* [https://www.linode.com/docs/applications/configuration-management/create-terraform-module/](https://www.linode.com/docs/applications/configuration-management/create-terraform-module/)
* [https://www.nearform.com/blog/writing-reusable-terraform-modules/](https://www.nearform.com/blog/writing-reusable-terraform-modules/)

