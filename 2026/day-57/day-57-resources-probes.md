# Day 57 – Resource Requests, Limits, and Probes

## What I Learned

Today I learned how Kubernetes manages container resources and monitors application health.

---

# Resource Requests and Limits

Created Pod with:

- CPU request: 100m
- Memory request: 128Mi
- CPU limit: 250m
- Memory limit: 256Mi

Verified using:

kubectl describe pod <pod-name>

Observed:

- Requests section
- Limits section
- QoS Class

QoS Class:

Burstable

Learned:

- Requests = scheduling minimum
- Limits = runtime maximum

---

# OOMKilled

Created Pod using:

polinux/stress

Configured container to allocate:

200M memory

while limit was:

100Mi

Observed:

Reason: OOMKilled
Exit Code: 137

Learned:

- CPU overuse → throttled
- Memory overuse → container killed

---

# Pending Pod

Created Pod requesting:

- cpu: 100
- memory: 128Gi

Observed:

Pod remained Pending

Verified using:

kubectl describe pod <pod-name>

Learned:

scheduler prevents Pods from running when resources are unavailable

---

# Liveness Probe

Created BusyBox Pod.

Generated file:

/tmp/healthy

Deleted file after 30 seconds.

Configured liveness probe using:

exec
cat /tmp/healthy

Observed:

container restarted automatically after probe failures

---

# Readiness Probe

Created nginx Pod with readiness probe.

Exposed Pod using Service.

Deleted:

/usr/share/nginx/html/index.html

Observed:

- Pod became 0/1 READY
- endpoints became empty
- container was not restarted

Learned:

readiness controls traffic routing

---

# Startup Probe

Created slow-starting container using:

sleep 20

Configured startup probe checking:

/tmp/started

Learned:

startup probes prevent unnecessary restarts during application startup

---

# Important Commands

kubectl describe pod
kubectl get pod -w
kubectl logs
kubectl exec
kubectl get endpoints

---

# Key Learning

Kubernetes uses:

- requests for scheduling
- limits for enforcement
- probes for health monitoring

Probe types:

- Liveness → restart container
- Readiness → control traffic
- Startup → allow slow startup
