# Day 62 – Providers, Resources and Dependencies

## What I Learned

Today I learned how Terraform manages infrastructure dependencies automatically.

---

# AWS Provider

Configured:

- AWS provider
- version constraint (~> 5.0)

Learned:

- provider plugins are downloaded during terraform init
- .terraform.lock.hcl locks provider versions

---

# VPC Infrastructure

Created:

- VPC
- Public Subnet
- Internet Gateway
- Route Table
- Route Table Association

Verified resources successfully in AWS Console.

---

# Implicit Dependencies

Examples:

Subnet → VPC

Internet Gateway → VPC

Route Table → VPC

Route Table Association → Route Table + Subnet

Learned:

Terraform automatically detects dependencies through resource references.

---

# Security Group

Configured:

- SSH (22)
- HTTP (80)
- Outbound traffic

Attached Security Group to EC2 instance.

---

# EC2 Instance

Created:

- Amazon Linux 2 instance
- t2.micro
- Public IP

Verified instance running successfully.

---

# Explicit Dependencies

Created S3 bucket using:

depends_on

Learned:

Terraform waits for dependency resources before creating dependent resources.

---

# Dependency Graph

Generated using:

terraform graph

Learned:

Graph shows resource creation relationships.

---

# Lifecycle Rules

Configured:

create_before_destroy = true

Learned:

- create_before_destroy
- prevent_destroy
- ignore_changes

---

# Destroy Infrastructure

Executed:

terraform destroy

Observed:

Terraform destroys resources in reverse dependency order.

---

# Important Commands

terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
terraform destroy
terraform graph

---

# Key Learning

Terraform automatically manages resource dependencies and creation order using references and dependency graphs.
