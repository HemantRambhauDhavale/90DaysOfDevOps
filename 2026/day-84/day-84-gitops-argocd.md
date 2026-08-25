# Day 84 — GitOps and ArgoCD

## What I Learned

Today I learned about **GitOps** and how ArgoCD can be used to manage Kubernetes deployments.

Earlier, I was using:

```bash
kubectl apply -f deployment.yaml
```

That works, but it doesn't answer an important question:

> Is the configuration running in Kubernetes still the same as the configuration stored in Git?

GitOps solves this by making Git the **source of truth**.

ArgoCD continuously compares the desired state in Git with the actual state in Kubernetes.

```text
Git
 |
 | Desired State
 v
ArgoCD
 |
 | Reconciliation
 v
Kubernetes / EKS
 |
 v
Application
```

---

## Why I Learned This

I am learning GitOps because DevOps is not only about deploying an application.

I also need to understand:

* How deployment changes are controlled
* How configuration is versioned
* How drift is detected
* How changes can be audited
* How rollback can be performed
* How manual changes can be controlled
* How developers can deploy without directly accessing the cluster

GitOps provides a structured way to manage these problems.

---

## How I Learned It

For the AI-BankApp project, I:

1. Verified ArgoCD on EKS.
2. Accessed the ArgoCD UI.
3. Studied the ArgoCD Application manifest.
4. Connected ArgoCD with the Git repository.
5. Deployed the application through ArgoCD.
6. Explored the application resource tree.
7. Checked sync and health status.
8. Checked ArgoCD sync history.
9. Tested configuration drift.
10. Observed ArgoCD self-healing.

---

## Four GitOps Principles

### 1. Declarative

The desired state is defined declaratively.

For Kubernetes, this is commonly represented using YAML manifests.

### 2. Versioned and Immutable

The desired configuration is stored in Git.

This provides:

* Version history
* Audit trail
* Pull requests
* Code review
* Rollback through Git

### 3. Pulled Automatically

ArgoCD pulls the desired state from Git.

The CI pipeline does not need to directly push every deployment into Kubernetes.

### 4. Continuously Reconciled

ArgoCD continuously compares:

```text
Desired State
      ↓
     Git
      ↓
     ArgoCD
      ↓
Actual State
      ↓
  Kubernetes
```

If there is a difference, ArgoCD can reconcile the state.

---

## GitOps vs Traditional CI/CD

| Aspect          | Traditional CI/CD                       | GitOps                        |
| --------------- | --------------------------------------- | ----------------------------- |
| Deployment      | CI pipeline can run deployment commands | ArgoCD reconciles Git state   |
| Source of truth | Pipeline/configuration                  | Git repository                |
| Drift detection | Usually limited                         | Continuous reconciliation     |
| Rollback        | Re-run pipeline/manual restore          | Git revert                    |
| Audit trail     | Pipeline logs                           | Git + ArgoCD history          |
| Cluster access  | CI may require cluster credentials      | ArgoCD manages cluster access |
| Desired state   | Can be spread across scripts            | Explicitly stored in Git      |

---

## AI-BankApp GitOps Flow

```text
Developer pushes code
        |
        v
GitHub Actions
        |
        +--> Build Maven project
        +--> Run tests
        +--> Build Docker image
        +--> Push image to DockerHub
        +--> Update Kubernetes image tag
        +--> Commit change to Git
        |
        v
Git Repository
        |
        v
ArgoCD
        |
        +--> Detect Git change
        +--> Compare desired and actual state
        +--> Sync Kubernetes resources
        +--> Reconcile drift
        |
        v
Amazon EKS
        |
        v
AI-BankApp
```

---

## ArgoCD Application Manifest

Important configuration:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: bankapp
  namespace: argocd

spec:
  project: default

  source:
    repoURL: https://github.com/<your-username>/AI-BankApp-DevOps.git
    targetRevision: feat/gitops
    path: k8s

  destination:
    server: https://kubernetes.default.svc
    namespace: bankapp

  syncPolicy:
    automated:
      prune: true
      selfHeal: true

    syncOptions:
      - CreateNamespace=true
      - ServerSideApply=true
```

---

## Manifest Field Explanation

### `repoURL`

The Git repository containing the Kubernetes manifests.

### `targetRevision`

The Git branch or revision ArgoCD watches.

In this project:

```text
feat/gitops
```

### `path`

The directory containing Kubernetes manifests.

```text
k8s
```

### `destination.server`

The Kubernetes cluster where ArgoCD deploys the resources.

```text
https://kubernetes.default.svc
```

### `destination.namespace`

The namespace where the application resources are deployed.

```text
bankapp
```

### `automated`

Enables automatic synchronization.

### `prune: true`

If a resource is removed from Git, ArgoCD can remove the corresponding resource from the cluster.

### `selfHeal: true`

If a supported manual change causes drift between Git and the cluster, ArgoCD can reconcile the resource toward the Git-defined state.

### `CreateNamespace=true`

Allows ArgoCD to create the target namespace if it does not already exist.

### `ServerSideApply=true`

Uses Kubernetes server-side apply for resource management and field ownership.

---

## Commands I Practiced

```bash
kubectl get pods -n argocd
```

```bash
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d && echo
```

```bash
kubectl port-forward svc/argocd-server -n argocd 8443:443
```

```bash
argocd version --client
```

```bash
argocd login localhost:8443 \
  --username admin \
  --password <your-password> \
  --insecure
```

```bash
argocd app get bankapp
```

```bash
argocd app wait bankapp
```

```bash
argocd app history bankapp
```

```bash
kubectl get pods -n bankapp -w
```

---

## Understanding Drift

Drift means the desired state and actual state are different.

```text
Git
 |
 | Desired State
 |
 | 4 replicas
 |
 X
 |
 | Actual State
 |
Kubernetes
 |
 | 1 replica
```

This means:

```text
Desired State != Actual State
```

ArgoCD detects this difference.

---

## Self-Healing Test

I tested what happens when Kubernetes is manually changed.

For example:

```bash
kubectl scale deployment bankapp \
  -n bankapp \
  --replicas=1
```

The idea was to create a difference between the Git configuration and the live Kubernetes configuration.

Another test was deleting a ConfigMap:

```bash
kubectl delete configmap bankapp-config -n bankapp
```

ArgoCD could detect the missing resource and reconcile it according to the desired state.

The important concept is:

```text
Manual Change
     ↓
Drift
     ↓
ArgoCD detects drift
     ↓
Reconciliation
     ↓
Desired state restored
```

---

## What I Learned From This

Before GitOps, I mostly thought about deployment like this:

```text
Build
 ↓
Docker
 ↓
kubectl apply
 ↓
Kubernetes
```

Now my understanding is:

```text
Code
 ↓
CI
 ↓
Git
 ↓
ArgoCD
 ↓
Kubernetes
 ↓
Continuous Reconciliation
```

This is a major improvement in my understanding of DevOps.

Deployment is not only about getting an application running.

We also need:

* Version control
* Auditability
* Consistency
* Drift detection
* Reconciliation
* Controlled changes
* Rollback capability

---

## Real-Life DevOps Connection

In a small student project, manually running:

```bash
kubectl apply
```

may be fine.

But imagine a production environment with:

* Multiple developers
* Multiple applications
* Multiple Kubernetes clusters
* Multiple environments
* Frequent deployments
* Security requirements
* Audit requirements

Manual changes become difficult to control.

GitOps provides a better model:

```text
Developer
   ↓
Git Pull Request
   ↓
Code Review
   ↓
Merge
   ↓
Git Desired State
   ↓
ArgoCD
   ↓
Kubernetes
```

---

## Interview Answer

If I am asked:

**What is GitOps?**

I can explain:

> GitOps is a deployment and operational model where Git stores the desired state of applications and infrastructure. Instead of a CI pipeline directly pushing deployment commands to Kubernetes, a tool such as ArgoCD watches Git and continuously reconciles the Kubernetes cluster with the desired state.

For my AI-BankApp project:

> GitHub Actions handles the CI process by building the application, creating the Docker image and updating the Kubernetes image tag in Git. ArgoCD then watches the repository and synchronizes the Kubernetes manifests to my EKS cluster. I also tested self-healing by manually changing Kubernetes resources and observing ArgoCD detect and reconcile the drift.

---

## My Day 84 Takeaway

The biggest thing I learned today is:

> **GitOps is not just about using ArgoCD. It is about managing the desired state of applications and infrastructure through Git and continuously reconciling that state with the actual environment.**

My new mental model:

```text
Git
 ↓
Desired State
 ↓
ArgoCD
 ↓
Continuous Reconciliation
 ↓
Kubernetes
```

---

## Day 84 Completed

* [x] Understood GitOps
* [x] Learned the four GitOps principles
* [x] Compared GitOps with traditional CI/CD
* [x] Explored ArgoCD
* [x] Studied the Application manifest
* [x] Understood automated sync
* [x] Understood `prune`
* [x] Understood `selfHeal`
* [x] Understood `ServerSideApply`
* [x] Deployed AI-BankApp through ArgoCD
* [x] Explored the ArgoCD resource tree
* [x] Checked sync history
* [x] Tested Kubernetes drift and reconciliation
