# Day 85 — ArgoCD Deep Dive

##  What I Learned Today

Today I went deeper into **ArgoCD** and learned how it can be used to manage Kubernetes applications in a more controlled and production-oriented way.

Until now, I mainly understood ArgoCD as a GitOps tool that continuously checks Git and keeps the Kubernetes cluster in the desired state.

Today I learned that ArgoCD provides much more than that.

I explored:

* Automated Sync
* Manual Sync
* Sync Waves
* Resource Ordering
* Rollbacks
* App of Apps Pattern
* Notifications
* Projects
* RBAC
* GitOps rollback using `git revert`

---

# 1. Automated Sync vs Manual Sync

ArgoCD can synchronize applications with Git in different ways.

## Automated Sync

With automated sync, ArgoCD automatically applies changes from Git to Kubernetes.

Example:

```yaml
syncPolicy:
  automated:
    prune: true
    selfHeal: true
```

### What this means

* Git changes can automatically reach Kubernetes.
* `prune: true` removes resources that were deleted from Git.
* `selfHeal: true` corrects manual changes made directly in the cluster.
* Less manual work is required.

### Where is it useful?

Automated sync can be very useful for:

* Development
* Testing
* Staging
* Applications where fast deployment is important

---

## Manual Sync

With manual sync, ArgoCD detects that the application is different from Git but waits for a person to approve the synchronization.

```yaml
syncPolicy: {}
```

The application can become:

```text
OutOfSync
```

but ArgoCD will not automatically apply the change.

A developer or DevOps engineer can review the difference first.

```bash
argocd app diff bankapp
```

Then sync manually:

```bash
argocd app sync bankapp
```

### Why is this useful?

Manual sync can be useful in production environments where we want:

```text
Git Change
    ↓
Review
    ↓
Approval
    ↓
Sync
    ↓
Production
```

This gives the team more control before making production changes.

---

# 2. Sync Waves

One of the most useful concepts I learned today was **Sync Waves**.

Sometimes Kubernetes resources have dependencies.

For example, my AI-BankApp has resources such as:

```text
Namespace
Storage
PVC
ConfigMap
Secret
MySQL
Ollama
Services
BankApp
HPA
```

It would not make sense to start the application before the required infrastructure and database resources are ready.

ArgoCD Sync Waves allow us to define the order.

Example:

```yaml
metadata:
  annotations:
    argocd.argoproj.io/sync-wave: "1"
```

ArgoCD processes lower wave numbers first.

## My deployment order

| Sync Wave | Resources               | Purpose                           |
| --------- | ----------------------- | --------------------------------- |
| -2        | Namespace, StorageClass | Infrastructure                    |
| -1        | PVC, ConfigMap, Secret  | Configuration & Storage           |
| 0         | MySQL, Ollama, Services | Database, AI service & Networking |
| 1         | BankApp Deployment      | Main Application                  |
| 2         | HPA                     | Application Scaling               |

So the overall flow becomes:

```text
Wave -2
Infrastructure
      ↓
Wave -1
Configuration & Storage
      ↓
Wave 0
Database + Services
      ↓
Wave 1
Application
      ↓
Wave 2
Scaling
```

This makes the deployment process easier to understand and control.

---

# 3. ArgoCD Rollback

ArgoCD keeps synchronization history.

I can check the history using:

```bash
argocd app history bankapp
```

This allows me to see previous revisions.

If a new deployment causes a problem, ArgoCD can be used to go back to an earlier revision.

Example:

```bash
argocd app rollback bankapp 1
```

The important thing I learned is:

> ArgoCD rollback changes the Kubernetes cluster, but it does not change Git.

This is very important in GitOps.

---

# 4. ArgoCD Rollback vs Git Revert

These two concepts look similar, but they are different.

## ArgoCD Rollback

```text
ArgoCD
   ↓
Older Revision
   ↓
Kubernetes Cluster
```

The cluster is moved back to an older state.

But Git still contains the latest commit.

## Git Revert

```text
Git
 ↓
git revert
 ↓
New commit
 ↓
ArgoCD
 ↓
Kubernetes
```

Example:

```bash
git revert HEAD
git push
```

This creates a new Git commit that reverses the previous change.

### Which approach is better for GitOps?

For a proper GitOps workflow, **Git should remain the source of truth**.

Therefore, when permanently reverting a change, `git revert` is generally the better approach because:

* Git history remains correct
* The change is auditable
* ArgoCD sees the desired state from Git
* The cluster eventually matches Git again

So I learned:

```text
ArgoCD rollback
= Quick cluster recovery

git revert
= GitOps-correct permanent change
```

---

# 5. App of Apps Pattern

Managing one application is simple.

But in a real Kubernetes environment, there can be many applications.

For example:

```text
BankApp
Monitoring
Envoy Gateway
Logging
Payments
Frontend
Backend
```

Managing every ArgoCD Application separately can become difficult.

The **App of Apps pattern** solves this by creating one parent Application that manages multiple child Applications.

Architecture:

```text
                    Git Repository
                         |
                         ↓
                     root-app
                    /    |     \
                   /     |      \
                  ↓      ↓       ↓
             bankapp  monitoring envoy-gateway
                |         |          |
                ↓         ↓          ↓
             BankApp   Prometheus   Gateway
                       + Grafana
```

The parent application watches a directory such as:

```text
argocd-apps/
├── root-app.yaml
├── bankapp.yaml
├── monitoring.yaml
└── envoy-gateway.yaml
```

The root application reads these files and creates the child Applications.

To apply the root application:

```bash
kubectl apply -f argocd-apps/root-app.yaml
```

Then:

```bash
argocd app list
```

can show the applications managed by ArgoCD.

### Why is this useful?

Adding a new application becomes easier.

Instead of manually creating another ArgoCD Application, we can add another YAML file to Git.

This makes application management more declarative.

---

# 6. ArgoCD Notifications

Another concept I learned was **ArgoCD Notifications**.

In a real environment, DevOps engineers need to know when something happens.

For example:

* Deployment succeeded
* Deployment failed
* Application health became degraded
* Application drifted from the desired state

Notifications can send information about these events.

The basic idea is:

```text
ArgoCD Event
     ↓
Trigger
     ↓
Template
     ↓
Notification Service
     ↓
Slack / Webhook / Other Destination
```

Example trigger:

```yaml
trigger.on-sync-succeeded: |
  - when: app.status.operationState.phase in ['Succeeded']
    send: [app-sync-succeeded]
```

A failed deployment can have another trigger:

```yaml
trigger.on-sync-failed: |
  - when: app.status.operationState.phase in ['Error', 'Failed']
    send: [app-sync-failed]
```

This is useful because the team doesn't always need to keep watching the ArgoCD dashboard.

---

# 7. ArgoCD Projects

When multiple teams use the same ArgoCD installation, everyone should not automatically have access to everything.

ArgoCD Projects help control:

* Which repositories can be used
* Which clusters can be used
* Which namespaces can be targeted

For example, a BankApp team project can be restricted to:

```text
Repository:
AI-BankApp repository

Allowed namespaces:
bankapp
monitoring
```

The same team should not automatically be able to deploy into:

```text
kube-system
argocd
other-team namespaces
```

This provides better separation between teams.

---

# 8. RBAC

Projects control application boundaries, while **RBAC** controls what users or groups are allowed to do.

For example:

```text
Developer
   |
   ├── View application       ✅
   ├── Sync application       ✅
   └── Rollback application   ❌
```

A senior team member can be given rollback permission separately.

This follows the principle of giving users only the permissions they actually need.

---

# 9. What I Understood from Day 85

Before today, I mainly looked at ArgoCD as:

```text
Git → ArgoCD → Kubernetes
```

Now I understand it more like:

```text
                    Git
                     |
                     ↓
                  ArgoCD
                     |
        ┌────────────┼────────────┐
        ↓            ↓            ↓
     Sync        Rollback       Apps
        ↓                         ↓
   Sync Waves                App of Apps
        ↓
   Kubernetes
        |
        ↓
 Notifications
        |
        ↓
     Team
```

ArgoCD is not only a deployment tool.

It also helps with:

* Desired-state management
* Deployment ordering
* Recovery
* Application management
* Notifications
* Access control
* Multi-team Kubernetes environments

---

# 10. Why I Am Learning This

I am learning these concepts because simply knowing commands is not enough for a DevOps role.

I want to understand:

**What am I doing?**

**Why am I doing it?**

**How does it work?**

**Where would a DevOps engineer actually use it?**

For example, knowing:

```bash
argocd app sync
```

is useful.

But understanding **when to use manual sync, why production may need approval, how to order resources, how to recover from a bad deployment, and how to control team permissions** is much more important.

---

# 11. Real DevOps Connection

In a real company, there may be many Kubernetes applications and many teams.

A possible workflow can look like:

```text
Developer
    ↓
Git Commit
    ↓
Pull Request / Review
    ↓
Git Merge
    ↓
ArgoCD
    ↓
Sync Waves
    ↓
Kubernetes
    ↓
Health Check
    ↓
Notification
```

If something goes wrong:

```text
Problem
   ↓
Investigation
   ↓
Rollback / Git Revert
   ↓
ArgoCD
   ↓
Healthy Kubernetes Cluster
```

This is where GitOps becomes useful beyond simply deploying YAML files.

---

# 12. Key Takeaways

### Automated Sync

Automatically applies changes from Git.

### Manual Sync

Allows humans to review and approve synchronization.

### Sync Waves

Control the order in which Kubernetes resources are synchronized.

### Rollback

Can quickly move the cluster to an older ArgoCD revision.

### Git Revert

Creates a new Git commit that reverses a previous change and keeps Git as the source of truth.

### App of Apps

Uses one parent Application to manage multiple child Applications.

### Notifications

Inform teams about sync, failure, and health events.

### Projects

Limit where applications can deploy.

### RBAC

Controls what users and teams can do.

---

# 13. Final Learning

The biggest thing I learned today is that **GitOps is not simply "deploy Kubernetes from Git."**

It is about maintaining a controlled relationship between:

```text
Git
 ↓
Desired State
 ↓
ArgoCD
 ↓
Kubernetes
 ↓
Application Health
```

And when something goes wrong, the goal is not just to fix the cluster.

The goal is to bring the **desired state, Git history, and cluster state back into alignment**.

Day 85 completed. 

#90DaysOfDevOps #DevOps #GitOps #ArgoCD #Kubernetes #AWS
