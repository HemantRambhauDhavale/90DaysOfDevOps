# Day 56 – Kubernetes StatefulSets

## What I Learned

Today I learned how Kubernetes StatefulSets manage stateful applications.

---

# Deployment vs StatefulSet

| Feature | Deployment | StatefulSet |
|---|---|---|
| Pod Names | Random | Stable |
| Startup | Parallel | Ordered |
| Storage | Shared | Dedicated PVC per Pod |
| DNS | No stable hostname | Stable DNS |

---

# Problem with Deployments

Created Deployment with:
- 3 replicas

Observed:
- random Pod names
- Pod name changed after deletion

Learned:
- not suitable for databases

---

# Headless Service

Created Headless Service using:

```yaml
clusterIP: None
