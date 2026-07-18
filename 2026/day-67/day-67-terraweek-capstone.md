# Day 67 – TerraWeek Capstone

## What I Learned

Today I completed a multi-environment Terraform project using custom modules and Terraform Workspaces.

---

# Terraform Workspaces

Created three environments:

- dev
- staging
- prod

Learned:

- Separate state for every workspace
- Environment isolation
- Same codebase for multiple environments

---

# Project Structure

Organized the project using:

- providers.tf
- variables.tf
- outputs.tf
- locals.tf
- main.tf
- dev.tfvars
- staging.tfvars
- prod.tfvars

Created reusable modules:

- VPC Module
- Security Group Module
- EC2 Module

---

# Workspace-Aware Configuration

Used:

terraform.workspace

Automatically generated:

- Environment names
- Resource names
- Common tags

Example:

- terraweek-dev-server
- terraweek-staging-server
- terraweek-prod-server

---

# Environment-Specific Configuration

Created separate tfvars files.

Development:

- t2.micro
- SSH + HTTP

Staging:

- t2.small
- SSH + HTTP + HTTPS

Production:

- t3.small
- HTTP + HTTPS

Each environment used a different VPC CIDR.

---

# Custom Modules

Created reusable modules for:

- VPC
- Security Group
- EC2 Instance

Each module contained:

- variables.tf
- main.tf
- outputs.tf

---

# Terraform Best Practices

Learned:

- Organize Terraform projects properly
- Keep modules focused on one purpose
- Avoid hardcoded values
- Use variables and tfvars
- Use workspaces for multiple environments
- Tag all resources consistently
- Always run terraform plan before apply

---

# Important Commands

terraform workspace new

terraform workspace list

terraform workspace select

terraform workspace show

terraform init

terraform plan

terraform apply

terraform destroy

---

# TerraWeek Summary

| Day | Topics |
|------|--------|
| Day 61 | Infrastructure as Code, Terraform Basics |
| Day 62 | Providers, Resources, Dependencies |
| Day 63 | Variables, Outputs, Data Sources, Locals |
| Day 64 | State Management, Remote Backend, Drift |
| Day 65 | Custom Modules, Registry Modules |
| Day 66 | Amazon EKS with Terraform |
| Day 67 | Workspaces, Multi-Environment Infrastructure, Capstone |

---

# Key Learning

Terraform Workspaces and reusable modules make it possible to manage multiple isolated environments from a single codebase, making Infrastructure as Code scalable, maintainable, and production-ready.
