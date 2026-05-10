# Day 52 – Kubernetes Namespaces & Deployments

Today I learned Kubernetes Namespaces and Deployments.

---

## Namespaces

Namespaces help organize and isolate Kubernetes resources.

Created namespaces:
- dev
- staging
- production

Commands used:

```bash
kubectl create namespace dev
kubectl create namespace staging
kubectl get namespaces
kubectl get pods -A
