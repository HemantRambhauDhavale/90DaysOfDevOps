# Day 58 – Metrics Server and Horizontal Pod Autoscaler (HPA)

## What I Learned

Today I learned how Kubernetes automatically scales applications using Metrics Server and Horizontal Pod Autoscaler (HPA).

---

# Metrics Server

Installed Metrics Server.

Verified using:

kubectl top nodes
kubectl top pods -A

Learned:

- Metrics Server collects CPU and memory usage
- kubectl top shows real-time resource usage

---

# kubectl top

Practiced:

kubectl top nodes
kubectl top pods -A
kubectl top pods -A --sort-by=cpu

Observed:

- node CPU usage
- node memory usage
- highest CPU-consuming Pods

---

# Deployment with CPU Requests

Created Deployment using:

registry.k8s.io/hpa-example

Configured:

resources.requests.cpu: 200m

Learned:

HPA requires CPU requests for utilization calculations.

---

# Horizontal Pod Autoscaler (HPA)

Created HPA using:

kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10

Verified using:

kubectl get hpa
kubectl describe hpa php-apache

Observed:

TARGETS initially showed <unknown>

After metrics arrived:
- CPU utilization displayed correctly

---

# Load Testing and Auto Scaling

Created BusyBox load generator.

Observed:

- CPU usage increased
- replicas scaled automatically
- Deployment scaled from 1 to multiple replicas

Deleted load generator.

Learned:

Kubernetes slowly scales Pods back down after traffic decreases.

---

# Declarative HPA

Created HPA using:

autoscaling/v2

Learned:

behavior section controls:
- scale-up speed
- scale-down stabilization

---

# autoscaling/v1 vs v2

| Feature | v1 | v2 |
|---|---|---|
| CPU Metrics | Yes | Yes |
| Memory Metrics | No | Yes |
| Custom Metrics | No | Yes |
| Advanced Behavior | No | Yes |

---

# Important Commands

kubectl top nodes
kubectl top pods -A
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
kubectl get hpa
kubectl describe hpa
kubectl get hpa --watch

---

# Key Learning

Kubernetes HPA automatically scales applications based on resource utilization.

Metrics Server provides the usage data required for HPA to work properly.
