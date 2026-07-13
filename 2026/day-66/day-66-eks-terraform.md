# Day 66 – Provision an AWS EKS Cluster with Terraform

## What I Learned

Today I learned how to provision a complete Amazon EKS cluster using Terraform registry modules.

---

# Project Structure

Created:

- providers.tf
- variables.tf
- terraform.tfvars
- vpc.tf
- eks.tf
- outputs.tf

Learned how to organize Terraform projects into separate configuration files.

---

# VPC Module

Created AWS networking using the Terraform AWS VPC module.

Resources included:

- VPC
- Public Subnets
- Private Subnets
- NAT Gateway
- Internet Gateway
- Route Tables

Learned why EKS requires both public and private subnets.

---

# EKS Module

Provisioned an Amazon EKS cluster using the official Terraform EKS module.

Terraform automatically created:

- EKS Control Plane
- Managed Node Group
- IAM Roles
- Security Groups
- Networking Components

---

# Connecting kubectl

Configured access using:

aws eks update-kubeconfig

Verified cluster using:

- kubectl get nodes
- kubectl get pods -A
- kubectl cluster-info

Confirmed worker nodes were in the Ready state.

---

# Deploying Nginx

Created:

- Deployment
- LoadBalancer Service

Verified:

- Running Pods
- External LoadBalancer
- Accessible Nginx Welcome Page

---

# Destroying Infrastructure

Deleted Kubernetes resources.

Executed:

terraform destroy

Verified removal of:

- EKS Cluster
- Node Groups
- VPC
- NAT Gateway
- Security Groups
- IAM Resources

---

# Important Commands

terraform init

terraform plan

terraform apply

aws eks update-kubeconfig

kubectl get nodes

kubectl get pods -A

kubectl get svc

kubectl apply -f nginx-deployment.yaml

terraform destroy

---

# Key Learning

Terraform registry modules make it possible to provision a complete production-style Amazon EKS cluster with reusable Infrastructure as Code, reducing manual configuration and simplifying Kubernetes deployment on AWS.
