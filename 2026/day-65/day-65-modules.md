# Day 65 – Terraform Modules

## What I Learned

Today I learned how Terraform Modules help organize and reuse infrastructure code.

---

# Terraform Modules

Learned:

- Root Module
- Child Module

A root module calls one or more child modules to create infrastructure.

---

# Custom EC2 Module

Created a reusable EC2 module with:

- AMI ID
- Instance Type
- Subnet ID
- Security Group IDs
- Instance Name
- Tags

Outputs:

- Instance ID
- Public IP
- Private IP

---

# Custom Security Group Module

Created a reusable Security Group module.

Used:

- Dynamic Ingress Rules
- Variable Ports
- Tags

Output:

- Security Group ID

---

# Reusing Modules

Called the EC2 module twice.

Created:

- Web Server
- API Server

Both instances used the same module with different input values.

---

# Terraform Registry Module

Used the official AWS VPC module.

Learned:

- Registry Modules
- Module Source
- Module Versioning

Verified that Terraform downloads registry modules into:

.terraform/modules/

---

# Module Versioning

Practiced:

- version = "5.1.0"
- version = "~> 5.0"

Learned why version pinning is important.

---

# Important Commands

terraform init

terraform plan

terraform apply

terraform destroy

terraform state list

terraform init -upgrade

---

# Module Best Practices

- Keep modules focused on a single purpose.
- Avoid hardcoded values by using variables.
- Always define outputs.
- Pin module versions.
- Add documentation for every module.

---

# Key Learning

Terraform Modules make Infrastructure as Code reusable, organized, and easier to maintain across multiple environments.
