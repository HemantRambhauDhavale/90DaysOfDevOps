# Day 78 – Introduction to Helm and Chart Basics

## What I Learned

Today I learned the fundamentals of Helm and how it simplifies Kubernetes application deployment using reusable charts.

---

# Helm Basics

Learned:

- Helm Installation
- Helm Charts
- Releases
- Repositories
- Values Files

Understood why Helm is called the package manager for Kubernetes.

---

# AI-BankApp Exploration

Explored the project structure.

Observed multiple Kubernetes manifests:

- Deployments
- Services
- ConfigMaps
- Secrets
- Persistent Volumes
- Persistent Volume Claims
- HPA

Learned why managing many YAML files manually becomes difficult.

---

# Deploying MySQL with Helm

Deployed MySQL using the Bitnami Helm Chart.

Helm automatically created:

- Deployment
- Service
- Secrets
- Persistent Volume Claim
- Storage Configuration

Verified:

- MySQL Pod Running
- Database Created Successfully

---

# Values File

Created:

- mysql-values.yaml

Configured:

- Root Password
- Database Name
- CPU Requests
- CPU Limits
- Memory Requests
- Memory Limits
- Persistent Storage
- Metrics

Learned that values files make deployments easier to manage.

---

# Helm Release Management

Practiced:

- helm install
- helm list
- helm history
- helm upgrade
- helm rollback
- helm uninstall

Verified:

- Release Installed
- Upgrade Successful
- Rollback Successful
- Revision History Maintained

---

# Chart Structure

Explored:

- Chart.yaml
- values.yaml
- templates/
- charts/
- _helpers.tpl
- NOTES.txt

Learned:

- Chart.yaml stores chart metadata
- values.yaml stores default configuration
- templates contain reusable Kubernetes manifests

---

# Raw YAML vs Helm

Raw Kubernetes Manifests:

- Multiple YAML files
- Manual updates
- Manual rollback
- Difficult environment management

Helm Charts:

- Single reusable package
- Easy customization
- Version controlled
- Built-in rollback
- Environment-specific values

---

# Important Commands

helm version

helm repo add

helm repo update

helm search repo

helm install

helm list

helm show values

helm history

helm upgrade

helm rollback

helm uninstall

helm pull

---

# Verification

Verified:

- Helm Installed Successfully
- Kubernetes Cluster Connected
- MySQL Deployed Successfully
- Helm Release Created
- Release History Available
- Chart Structure Explored

---

# Key Learning

Today I learned how Helm simplifies Kubernetes deployments by packaging multiple resources into reusable charts. Using Helm makes application deployment, configuration management, upgrades, and rollbacks much easier compared to managing raw Kubernetes YAML files.
