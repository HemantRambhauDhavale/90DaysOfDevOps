# Day 82 — EKS Networking with Gateway API and Persistent Storage

## What I Learned

Today I went deeper into **EKS networking and persistent storage** using the AI-BankApp.

Until now, I had mainly focused on deploying applications into Kubernetes. Today I understood what happens when we want to expose that application properly, handle HTTPS, maintain user sessions and store application data reliably.

---

## 1. Gateway API

The AI-BankApp uses the **Kubernetes Gateway API** instead of a traditional Ingress resource.

The basic traffic flow is:

```text
Internet
   |
AWS NLB
   |
Gateway
   |
HTTPRoute
   |
Service
   |
Pods
```

The important Gateway API resources I learned were:

### GatewayClass

Defines which controller will manage the Gateway.

```yaml
kind: GatewayClass
```

In this project, Envoy Gateway is responsible for managing it.

### Gateway

Defines the actual entry point for traffic.

It can contain listeners such as:

* HTTP — port 80
* HTTPS — port 443

The HTTPS listener can also use a TLS certificate.

### HTTPRoute

Defines where the incoming request should go.

For the AI-BankApp:

```text
Gateway
   ↓
HTTPRoute
   ↓
bankapp-service
   ↓
BankApp Pods
```

### BackendTrafficPolicy

This is an Envoy Gateway resource used in this project for backend traffic behaviour.

I used it to understand **cookie-based session persistence**.

---

## 2. Gateway API vs Ingress

| Feature           | Ingress                               | Gateway API                       |
| ----------------- | ------------------------------------- | --------------------------------- |
| Main purpose      | HTTP/HTTPS routing                    | Advanced traffic management       |
| Traffic splitting | Usually implementation-specific       | Built into the API                |
| Header matching   | Often annotation/controller dependent | Native routing rules              |
| Role separation   | Less defined                          | GatewayClass → Gateway → Route    |
| TLS               | Commonly controller/annotation based  | Defined through Gateway listeners |
| Extensibility     | More limited                          | More expressive                   |

The main thing I understood is that Gateway API separates responsibilities better.

Infrastructure teams can manage the GatewayClass and Gateway, while application teams can manage their HTTPRoutes.

---

## 3. Envoy Gateway

I installed Envoy Gateway using Helm.

After installation, I checked:

```bash
kubectl get pods -n envoy-gateway-system
kubectl get gatewayclass
```

The important thing I was looking for was the Envoy Gateway controller and the GatewayClass.

When the Gateway was created, Envoy Gateway could provision the external AWS load-balancer integration.

---

## 4. Session Persistence

This was one of the most important concepts for me today.

The BankApp can run multiple replicas:

```text
             ┌── Pod 1
User → Gateway
             └── Pod 2
```

Suppose a user logs in through Pod 1.

If the next request goes to Pod 2 and the session is not available there, the user can face authentication/session problems.

The project therefore uses cookie-based affinity.

```text
User
 |
BANKAPP_AFFINITY cookie
 |
Gateway
 |
Same backend pod
```

The important point I learned is that **scaling an application horizontally can introduce session-management problems**.

---

## 5. TLS with cert-manager

I also explored how HTTPS certificates can be automated using cert-manager and Let's Encrypt.

The basic flow is:

```text
cert-manager
      |
      ↓
Let's Encrypt
      |
HTTP-01 Challenge
      |
Certificate Issued
      |
Kubernetes Secret
      |
Gateway HTTPS Listener
```

Instead of manually creating and renewing certificates, cert-manager can manage the certificate lifecycle.

For quick testing, the project uses `nip.io`, which can resolve a hostname based on an IP address.

---

## 6. Persistent Storage with EBS

I also worked with EKS persistent storage.

The storage flow is:

```text
StorageClass
     ↓
PVC
     ↓
PV
     ↓
EBS Volume
     ↓
Pod
```

The AI-BankApp uses EBS-backed storage for:

* MySQL
* Ollama

I checked:

```bash
kubectl get storageclass gp3
kubectl get pvc -n bankapp
kubectl get pv
```

The PVC should eventually become:

```text
STATUS: Bound
```

That means Kubernetes successfully associated the PVC with a persistent volume.

---

## 7. Why EBS Persistence Matters

A pod is temporary.

If the MySQL pod is deleted, Kubernetes can create a new MySQL pod.

But the database data should not disappear with the pod.

The data is stored on the persistent EBS volume.

I tested this concept by deleting the MySQL pod and checking the database again.

```bash
kubectl delete pod -n bankapp -l app=mysql
```

After the pod was recreated, the database could still use the existing persistent storage.

This helped me understand the difference between:

```text
Pod = compute/application instance

EBS = persistent data storage
```

---

## 8. Important EBS Concepts

### gp3

`gp3` is an EBS volume type commonly used for general-purpose SSD storage.

### ReadWriteOnce

The volume can be mounted for read/write access by one node at a time.

This is important when using EBS because EBS volumes are tied to an Availability Zone.

### WaitForFirstConsumer

This can delay volume provisioning until Kubernetes knows where the pod will run.

That helps Kubernetes create the EBS volume in a suitable Availability Zone.

### Volume Expansion

The StorageClass can allow the volume size to be increased without recreating the entire storage setup.

---

## 9. HPA and Resource Usage

I also checked the application's Horizontal Pod Autoscaler.

```bash
kubectl get hpa -n bankapp
```

And checked resource usage:

```bash
kubectl top nodes
kubectl top pods -n bankapp
```

The important observation was that Ollama consumes considerably more resources than the normal BankApp pods.

So when designing an EKS cluster, it is not enough to think only about the number of pods.

We also need to think about:

* CPU requests
* Memory requests
* Number of replicas
* System pod overhead
* Node capacity

---

## 10. Architecture I Understood Today

### Networking

```text
                    Internet
                       |
                    AWS NLB
                       |
               Envoy Gateway
                       |
                  HTTPRoute
                       |
                 bankapp-service
                       |
                ┌──────┴──────┐
                |             |
             Pod 1          Pod 2
```

### Storage

```text
StorageClass
     |
     ↓
   PVC
     |
     ↓
   PV
     |
     ↓
 AWS EBS
     |
     ↓
 MySQL / Ollama Pod
```

---

## 11. Key Takeaways

Today I understood:

* Gateway API provides a more expressive way to manage Kubernetes traffic.
* Envoy Gateway implements Gateway API resources.
* GatewayClass defines the controller.
* Gateway defines listeners and the entry point.
* HTTPRoute defines application routing.
* Cookie-based affinity can help stateful web sessions work correctly across multiple pods.
* cert-manager can automate TLS certificates.
* EBS provides persistent storage for workloads running on EKS.
* PVCs request storage while PVs represent the provisioned storage.
* EBS volumes have Availability Zone considerations.
* HPA and resource requests are important when planning EKS capacity.

---

## Day 82 Summary

The biggest thing I learned today was that **Kubernetes networking and storage are not just about exposing a pod or attaching a disk**.

A production application needs:

```text
Traffic Management
        +
HTTPS
        +
Session Management
        +
Persistent Storage
        +
Resource Management
```

Understanding how these pieces work together makes the Kubernetes deployment much more meaningful than simply running `kubectl apply`.

**Day 82 completed. 🚀**

#90DaysOfDevOps #DevOps #AWS #EKS #Kubernetes #GatewayAPI #EnvoyGateway
