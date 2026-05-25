# Day 60 – Kubernetes Capstone Project

## What I Learned

Today I deployed a complete WordPress + MySQL application using multiple Kubernetes concepts together.

---

# Namespace

Created namespace:

capstone

Configured default namespace context.

---

# MySQL StatefulSet

Created:

- Secret
- Headless Service
- StatefulSet
- Persistent Volume Claims

Configured:

- mysql:8.0 image
- resource requests and limits
- persistent storage

Verified:

wordpress database existed successfully

---

# WordPress Deployment

Created Deployment with:

- 2 replicas
- wordpress:latest image

Used:

- ConfigMap
- Secret references
- resource limits
- liveness probe
- readiness probe

Verified:

both Pods reached 1/1 Running

---

# NodePort Service

Created NodePort Service.

Accessed WordPress from browser.

Completed setup wizard successfully.

---

# Self-Healing Test

Deleted:

- WordPress Pod
- MySQL Pod

Observed:

- Kubernetes recreated Pods automatically
- WordPress blog data remained safe

Learned:

Deployments and StatefulSets provide self-healing.

PVCs preserve data after Pod recreation.

---

# Horizontal Pod Autoscaler

Created HPA with:

- min replicas: 2
- max replicas: 10
- CPU target: 50%

Verified using:

kubectl get hpa

---

# Helm Comparison

Installed WordPress using Helm.

Compared:

- manual YAML deployment
- Helm deployment

Learned:

- Helm simplifies deployments
- manual YAML provides more control

---

# Kubernetes Concepts Used

| Concept | Purpose |
|---|---|
| Namespace | Resource isolation |
| Secret | Store passwords |
| ConfigMap | Store configuration |
| StatefulSet | Stable database Pods |
| PVC | Persistent storage |
| Headless Service | Stable DNS |
| Deployment | WordPress replicas |
| Service | Network access |
| Probes | Health checks |
| HPA | Auto scaling |
| Helm | Package management |

---

# Important Commands

kubectl get all -n capstone
kubectl get pvc
kubectl get hpa
kubectl describe pod
kubectl logs
kubectl delete pod
helm install

---

# Key Learning

This capstone project combined multiple Kubernetes concepts into one real-world application deployment with self-healing, persistence, networking, and scaling.
