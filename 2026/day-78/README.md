# Day 78 - Introduction to Helm and Chart Basics

##  Objective

Today I started learning **Helm**, the package manager for Kubernetes. Instead of managing many Kubernetes YAML files manually, Helm allows us to package and deploy applications using reusable charts.

---

# What I Learned Today

* What Helm is
* Why Helm is used in Kubernetes
* Helm Chart
* Helm Release
* Helm Repository
* Helm Values
* Installing Helm
* Deploying MySQL using Bitnami Helm Chart
* Using `values.yaml`
* Helm Upgrade
* Helm Rollback
* Helm Uninstall
* Helm Chart Structure

---

# What is Helm?

Helm is a package manager for Kubernetes.

Just like we use **apt** in Ubuntu or **yum** in RHEL to install software, Helm helps us install and manage Kubernetes applications.

Instead of writing many Kubernetes YAML files manually, Helm packages everything into a reusable chart.

---

# Core Concepts

## Chart

A Chart is a package that contains all Kubernetes resource templates needed to deploy an application.

Example:

* Deployment
* Service
* ConfigMap
* Secret
* PVC

All are packaged together inside one chart.

---

## Release

A Release is a running instance of a Helm Chart inside the Kubernetes cluster.

The same chart can be installed multiple times using different release names.

---

## Repository

A Repository is a place where Helm Charts are stored.

Example:

* Bitnami Repository
* Artifact Hub

---

## Values

Values are configuration settings used to customize a chart without changing its templates.

Example:

* Image Tag
* Replicas
* Resources
* Passwords
* Storage Size

---

# Why Helm Instead of Raw YAML?

Without Helm:

* Many YAML files
* Manual configuration
* Hard to reuse
* Difficult rollback

With Helm:

* Single reusable chart
* Easy configuration
* Version control
* Rollback support
* Faster deployment

---

# Practical Work

Today I:

* Installed Helm
* Connected Helm with Kubernetes
* Added Bitnami Repository
* Updated Helm repositories
* Searched MySQL chart
* Installed MySQL using Helm
* Created a custom `mysql-values.yaml`
* Upgraded Helm Release
* Rolled back to previous revision
* Explored Helm Chart structure

---

# Helm Chart Structure

```text
Chart.yaml
values.yaml
templates/
charts/
_helpers.tpl
NOTES.txt
```

### Chart.yaml

Contains chart metadata like:

* Name
* Version
* Description
* App Version

### values.yaml

Contains default configuration values.

### templates/

Contains Kubernetes resource templates.

### charts/

Contains chart dependencies.

---

# Difference Between Version and AppVersion

**Version**

Represents the version of the Helm Chart.

**AppVersion**

Represents the version of the application running inside the chart.

---

# My Key Learning

Helm does not replace Kubernetes.

It makes Kubernetes deployments easier, reusable, and easier to manage.

---

# Commands Practiced

```bash
helm version

helm repo add bitnami https://charts.bitnami.com/bitnami

helm repo update

helm search repo bitnami/mysql

helm install

helm list

helm history

helm upgrade

helm rollback

helm uninstall
```

---

# Takeaway

Today I understood why Helm is one of the most important tools in Kubernetes.

Managing multiple YAML files manually is possible, but Helm makes deployments cleaner, reusable, and much easier to maintain across different environments.

Tomorrow I'll start creating my own Helm Chart for the AI-BankApp project.

---

**Day 78 Completed ✅**
