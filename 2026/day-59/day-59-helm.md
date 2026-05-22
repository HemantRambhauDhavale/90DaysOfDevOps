# Day 59 – Helm — Kubernetes Package Manager

## What I Learned

Today I learned how Helm simplifies Kubernetes application deployment and management.

---

# Helm Core Concepts

Learned three main concepts:

- Chart
- Release
- Repository

Definitions:

- Chart → Kubernetes package
- Release → running installation of a chart
- Repository → collection of charts

---

# Installing Helm

Installed Helm.

Verified using:

helm version
helm env

---

# Bitnami Repository

Added Bitnami repository:

helm repo add bitnami https://charts.bitnami.com/bitnami

Updated repository:

helm repo update

Searched charts:

helm search repo nginx
helm search repo bitnami

---

# Installing a Chart

Installed nginx chart:

helm install my-nginx bitnami/nginx

Observed:

- Deployment created
- Service created
- Pods created automatically

Verified using:

kubectl get all
helm list
helm status my-nginx

---

# Customizing Values

Viewed default values:

helm show values bitnami/nginx

Customized:

- replicaCount
- service type
- resource limits

Used:

--set

and:

custom-values.yaml

Verified using:

helm get values <release-name>

---

# Upgrade and Rollback

Upgraded release:

helm upgrade my-nginx bitnami/nginx --set replicaCount=5

Checked history:

helm history my-nginx

Performed rollback:

helm rollback my-nginx 1

Learned:

rollback creates new revision history

---

# Creating Custom Chart

Created chart:

helm create my-app

Explored:

- Chart.yaml
- values.yaml
- templates/

Learned Helm template syntax:

{{ .Values.replicaCount }}
{{ .Chart.Name }}

Modified:

- replica count
- nginx image

Validated using:

helm lint my-app

Previewed manifests:

helm template my-release ./my-app

Installed chart:

helm install my-release ./my-app

---

# Important Commands

helm version
helm repo add
helm repo update
helm search repo
helm install
helm list
helm status
helm get manifest
helm upgrade
helm rollback
helm create
helm lint
helm template

---

# Key Learning

Helm simplifies Kubernetes application deployment by packaging multiple Kubernetes resources into reusable charts.
