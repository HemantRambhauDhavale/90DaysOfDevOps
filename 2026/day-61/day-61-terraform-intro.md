# Day 61 – Introduction to Terraform and AWS Infrastructure

## What I Learned

Today I started learning Terraform and Infrastructure as Code (IaC).

---

# Infrastructure as Code (IaC)

Learned:

- infrastructure can be managed using code
- avoids manual cloud configuration
- improves automation and consistency

Benefits:

- repeatable deployments
- version control
- reduced human error

---

# Installing Terraform

Installed:

- Terraform
- AWS CLI

Verified using:

terraform -version
aws sts get-caller-identity

---

# First Terraform Project

Created:

main.tf

Configured:

- AWS provider
- AWS region
- S3 bucket resource

Executed Terraform workflow:

terraform init
terraform plan
terraform apply

Observed:

S3 bucket created successfully in AWS.

---

# Understanding terraform init

Learned:

terraform init:
- downloads provider plugins
- creates .terraform directory
- initializes Terraform project

---

# Creating EC2 Instance

Added:

aws_instance

Configured:

- Amazon Linux 2 AMI
- t2.micro
- Name tag

Applied configuration successfully.

Verified EC2 instance from AWS Console.

---

# Terraform State File

Explored:

terraform.tfstate

Used commands:

terraform show
terraform state list
terraform state show

Learned:

- Terraform tracks infrastructure state
- state file stores resource details
- state files should not be committed to GitHub

---

# Modify and Destroy

Modified:

EC2 Name tag

Ran:

terraform plan

Learned Terraform symbols:

+ → create
~ → modify
- → destroy

Destroyed infrastructure using:

terraform destroy

Verified resources removed from AWS.

---

# Important Commands

terraform init
terraform plan
terraform apply
terraform destroy
terraform show
terraform state list
terraform state show
terraform fmt
terraform validate

---

# Key Learning

Terraform allows cloud infrastructure to be created, modified, and destroyed using code instead of manual AWS Console operations.
