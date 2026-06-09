# Day 63 – Variables, Outputs, Data Sources and Expressions

## What I Learned

Today I learned how to make Terraform configurations dynamic and reusable.

---

# Variables

Created:

variables.tf

Defined variables:

- region
- vpc_cidr
- subnet_cidr
- instance_type
- project_name
- environment
- allowed_ports
- extra_tags

Learned Terraform variable types:

- string
- number
- bool
- list
- map

---

# Variable Files

Created:

terraform.tfvars

and

prod.tfvars

Used different values for:

- development
- production

Learned:

same Terraform code can be reused across environments.

---

# Outputs

Created outputs for:

- vpc_id
- subnet_id
- instance_id
- instance_public_ip
- instance_public_dns
- security_group_id

Verified using:

terraform output
terraform output instance_public_ip
terraform output -json

---

# Data Sources

Used:

data "aws_ami"

to fetch latest Amazon Linux 2 AMI.

Used:

data "aws_availability_zones"

to fetch available AZs.

Learned:

Resources create infrastructure.

Data sources fetch existing information.

---

# Locals

Created:

locals {
  name_prefix = "${var.project_name}-${var.environment}"
}

Used:

merge()

for consistent tagging.

Learned:

locals reduce repeated code.

---

# Built-in Functions

Practiced:

upper()
join()
format()
length()
lookup()
cidrsubnet()

Used terraform console for testing.

---

# Conditional Expressions

Configured:

instance_type = var.environment == "prod" ? "t3.small" : "t2.micro"

Learned:

Terraform can create environment-specific configurations.

---

# Variable Precedence

Low → High Priority

1. Default values
2. terraform.tfvars
3. *.auto.tfvars
4. -var-file
5. TF_VAR_ environment variables
6. -var

---

# Important Commands

terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
terraform output
terraform console

---

# Key Learning

Variables, outputs, data sources, locals, and expressions make Terraform configurations reusable, flexible, and suitable for multiple environments.
