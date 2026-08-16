# Day 81 — Introduction to Amazon EKS with Terraform

## 🎯 Goal

Today I learned how to run Kubernetes on AWS using **Amazon EKS** and provision the infrastructure using **Terraform**.

Until now, Kubernetes was mostly running locally with Kind. EKS helped me understand how the same Kubernetes concepts work in a cloud environment.

---

## 📚 What I Learned

### 1. What is Amazon EKS?

Amazon EKS stands for **Elastic Kubernetes Service**.

It is AWS's managed Kubernetes service.

The important point I learned is:

> AWS manages the Kubernetes control plane, while I manage the worker nodes and applications running on them.

### 2. EKS Architecture

The basic architecture I understood today:

```text
                         AWS
                          |
                         VPC
                          |
              +-----------+-----------+
              |                       |
        Public Subnets          Private Subnets
              |                       |
        Load Balancer             EKS Nodes
                                      |
                              +-------+-------+
                              |       |       |
                            Node 1  Node 2  Node 3
                              |       |       |
                             Pods    Pods    Pods
```

The EKS environment also contains a managed control plane that communicates with the worker nodes.

---

## 🧩 Important EKS Components

### EKS Control Plane

The control plane contains important Kubernetes components such as:

* API Server
* etcd
* Scheduler
* Controller Manager

AWS manages these components for EKS.

### Managed Node Group

The worker nodes are EC2 instances where Kubernetes pods actually run.

For the AI-BankApp setup:

* Instance type: `t3.medium`
* Desired nodes: `3`
* Maximum nodes: `5`

### VPC

The EKS cluster runs inside an AWS VPC.

The Terraform configuration creates:

* Public subnets
* Private subnets
* Intra subnets
* Internet Gateway
* NAT Gateway

The subnets are distributed across multiple Availability Zones.

---

## 🔌 EKS Add-ons

I studied the main add-ons used by the project:

| Add-on                 | Purpose                                 |
| ---------------------- | --------------------------------------- |
| CoreDNS                | DNS resolution inside Kubernetes        |
| kube-proxy             | Network routing for Kubernetes Services |
| VPC CNI                | Gives pods AWS VPC networking           |
| EKS Pod Identity Agent | Pod-level AWS IAM permissions           |
| EBS CSI Driver         | Allows Kubernetes to use EBS storage    |
| Metrics Server         | Enables metrics, `kubectl top` and HPA  |

The **EBS CSI Driver** was especially important because the application needs persistent storage for components such as MySQL and Ollama.

---

# 🏗️ Terraform Configuration

I studied the Terraform directory of the AI-BankApp project.

```text
terraform/
│
├── argocd.tf
├── eks.tf
├── outputs.tf
├── provider.tf
├── terraform.tfvars
├── variables.tf
└── vpc.tf
```

### `variables.tf`

Contains the input variables used by Terraform.

Examples:

```text
aws_region
cluster_name
cluster_version
node_instance_type
node_desired_count
node_max_count
```

### `terraform.tfvars`

Contains the actual values for the environment.

For example:

```text
aws_region         = "us-west-2"
cluster_name       = "bankapp-eks"
node_instance_type = "t3.medium"
node_desired_count = 3
node_max_count     = 5
```

### `vpc.tf`

Creates the networking foundation.

It creates:

* VPC
* Public subnets
* Private subnets
* Intra subnets
* Internet Gateway
* NAT Gateway

The important thing I understood here is that networking comes before the Kubernetes workloads.

### `eks.tf`

This is the main EKS configuration.

It creates:

* EKS cluster
* Managed node group
* IAM roles
* EKS add-ons
* EBS CSI configuration

### `argocd.tf`

Installs ArgoCD using Helm.

ArgoCD will later be used for GitOps.

### `outputs.tf`

Provides useful information after Terraform finishes.

For example, it provides the command needed to connect `kubectl` with the EKS cluster.

---

# ⚙️ Important Commands

### Check tools

```bash
terraform --version
aws --version
kubectl version --client
helm version
```

### Check AWS identity

```bash
aws sts get-caller-identity
```

### Initialize Terraform

```bash
terraform init
```

### Review infrastructure

```bash
terraform plan
```

### Create infrastructure

```bash
terraform apply
```

### Check Terraform outputs

```bash
terraform output
```

---

# ☸️ Connect kubectl to EKS

After the cluster is created:

```bash
aws eks update-kubeconfig \
  --name bankapp-eks \
  --region us-west-2
```

Then:

```bash
kubectl config current-context
kubectl cluster-info
kubectl get nodes -o wide
```

The important thing I learned here is that creating an EKS cluster and connecting `kubectl` to it are two different steps.

---

# 🔍 Check Kubernetes Components

```bash
kubectl get pods -n kube-system
```

Check nodes:

```bash
kubectl get nodes -o wide
```

Check EBS CSI:

```bash
kubectl get pods -n kube-system \
  -l app.kubernetes.io/name=aws-ebs-csi-driver
```

Check metrics:

```bash
kubectl top nodes
```

---

# 🚀 ArgoCD

Terraform also installs ArgoCD through Helm.

Check it:

```bash
kubectl get pods -n argocd
kubectl get svc -n argocd
```

Get the initial password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d
```

Get the LoadBalancer hostname:

```bash
kubectl get svc -n argocd argocd-server \
  -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'
```

ArgoCD will become more important in the upcoming GitOps days.

---

# 💾 Persistent Storage

One important difference I noticed between local Kubernetes and AWS Kubernetes is persistent storage.

The AI-BankApp uses EBS volumes.

The EBS CSI Driver allows Kubernetes to dynamically work with AWS EBS storage.

I can check the storage using:

```bash
kubectl get pvc -n bankapp
kubectl get pv
```

---

# 💰 EKS Cost Understanding

EKS is not free.

The major cost components are:

* EKS control plane
* EC2 worker nodes
* NAT Gateway
* EBS volumes
* LoadBalancer
* Data transfer

The NAT Gateway was interesting because it has an hourly charge and can also generate data-processing charges.

So leaving a learning cluster running unnecessarily can become expensive.

---

# 🧹 Cleanup

When I don't need the workload:

```bash
kubectl delete -f k8s/
```

And when the complete infrastructure is no longer required:

```bash
terraform destroy
```

For cloud learning, cleanup is an important part of the process.

---

# 🧠 Key Takeaways

Today I understood:

* What managed Kubernetes means
* Difference between EKS control plane and worker nodes
* How EKS uses VPC networking
* How Managed Node Groups work
* Why EBS CSI Driver is required
* How IAM integrates with EKS
* How Terraform can create the complete infrastructure
* How `kubectl` connects to an EKS cluster
* How ArgoCD can be installed using Helm
* Why cloud cost management matters

### Day 81 Summary

```text
Terraform
    ↓
AWS VPC
    ↓
Subnets + Networking
    ↓
EKS Cluster
    ↓
Managed Node Group
    ↓
Kubernetes Pods
    ↓
AI-BankApp
```

## 📸 Evidence to Add

* [ ] `kubectl get nodes -o wide`
* [ ] `kubectl get pods -n kube-system`
* [ ] AI-BankApp running on EKS
* [ ] `kubectl get pvc -n bankapp`
* [ ] ArgoCD accessible
* [ ] EKS architecture diagram

## 🎯 Next

Next I will continue with the EKS environment and go deeper into the networking and Kubernetes infrastructure before moving further into the GitOps setup.
