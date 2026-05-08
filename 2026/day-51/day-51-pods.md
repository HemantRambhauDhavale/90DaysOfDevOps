# Day 51 – Kubernetes Pods & Manifests

Today I created my first Kubernetes Pods using YAML manifests.

---

## Kubernetes Manifest Structure

Required fields:

- apiVersion
- kind
- metadata
- spec

---

## First Pod – Nginx

Created:
- nginx-pod.yaml

Commands used:

```bash
kubectl apply -f nginx-pod.yaml
kubectl get pods
kubectl describe pod nginx-pod
kubectl logs nginx-pod
kubectl exec -it nginx-pod -- /bin/bash
