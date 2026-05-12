# Day 53 – Kubernetes Services

## What I Learned

Today I learned Kubernetes Services and how they solve networking problems between Pods and applications.

Pods are temporary and their IPs change whenever they restart. Services provide stable communication and load balancing.

---

# Deployment

Created:
- app-deployment.yaml

Deployment details:
- Nginx image
- 3 replicas
- Labels and selectors

Commands used:

```bash id="8q1dql"
kubectl apply -f app-deployment.yaml
kubectl get pods -o wide
```

---

# ClusterIP Service

Created:
- clusterip-service.yaml

Purpose:
- Internal communication inside cluster

Commands:

```bash id="kv9j9j"
kubectl apply -f clusterip-service.yaml
kubectl get services
```

Testing inside cluster:

```bash id="ry7uql"
kubectl run test-client --image=busybox:latest --rm -it --restart=Never -- sh
wget -qO- http://web-app-clusterip
```

---

# Kubernetes DNS

Learned service discovery using DNS.

Examples:

```bash id="gy4i9m"
web-app-clusterip
web-app-clusterip.default.svc.cluster.local
```

DNS testing:

```bash id="pt2d5p"
nslookup web-app-clusterip
```

---

# NodePort Service

Created:
- nodeport-service.yaml

Purpose:
- External access using Node IP and Port

Commands:

```bash id="i7c8y2"
kubectl apply -f nodeport-service.yaml
kubectl get services
```

Access:
```text id="3oqk6k"
<NodeIP>:30080
```

---

# LoadBalancer Service

Created:
- loadbalancer-service.yaml

Purpose:
- Public external access in cloud environments

Commands:

```bash id="pk8xy2"
kubectl apply -f loadbalancer-service.yaml
kubectl get services
```

Learned:
- Local clusters show EXTERNAL-IP as pending
- Cloud providers create real load balancers

---

# Important Learning

## ClusterIP
Internal cluster communication only.

## NodePort
External access through node IP and port.

## LoadBalancer
Production external traffic using cloud load balancer.

---

# Kubernetes Networking Flow

Client → Service → Pods

The Service distributes traffic across healthy Pods automatically.

---

# Commands Practiced

```bash id="b8r7ly"
kubectl get services
kubectl describe service web-app-clusterip
kubectl get endpoints
kubectl get pods -o wide
```

---

# Summary

Today I learned:
- Kubernetes Services
- Stable networking
- Service discovery
- DNS
- ClusterIP
- NodePort
- LoadBalancer
- Pod communication
