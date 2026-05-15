# Day 55 – Persistent Volumes (PV) & Persistent Volume Claims (PVC)

## What I Learned

Today I learned how Kubernetes handles persistent storage using Persistent Volumes and Persistent Volume Claims.

---

# The Problem – Ephemeral Storage

Created a Pod using:
- emptyDir volume

Stored data inside:
- /data/message.txt

After deleting and recreating the Pod:
- old data disappeared

Learned:
- Pod storage is temporary by default

---

# Persistent Volumes (PV)

Created a PersistentVolume using:
- 1Gi storage
- ReadWriteOnce
- Retain reclaim policy
- hostPath

Checked PV status:

```bash
kubectl get pv
