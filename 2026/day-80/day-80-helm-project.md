# Day 80 — Helm Multi-Environment Deployment and CI/CD

## What I Learned

Today I continued working with Helm and focused on using one Helm chart for multiple environments.

The main idea was:

```text
One Helm Chart
      |
      +---- Dev
      |
      +---- Staging
      |
      +---- Production
```

Instead of creating separate Kubernetes manifests for every environment, I can keep the same chart and change the configuration through different values files.

---

## 1. Environment-Specific Values

I created three values files:

```text
values-dev.yaml
values-staging.yaml
values-prod.yaml
```

Each file changes things like:

* Application replicas
* Docker image tag
* CPU and memory
* MySQL storage
* Ollama resources
* Autoscaling
* Gateway configuration

### Dev

Dev is configured with smaller resources and a single BankApp replica.

```yaml
bankapp:
  replicaCount: 1

mysql:
  persistence:
    size: 2Gi

ollama:
  persistence:
    size: 5Gi
```

Autoscaling is disabled in dev.

### Staging

Staging uses more resources and enables HPA.

```yaml
bankapp:
  autoscaling:
    enabled: true
    minReplicas: 2
    maxReplicas: 3
```

The image uses a specific version instead of `latest`.

### Production

Production has higher resources and storage.

```yaml
bankapp:
  autoscaling:
    enabled: true
    minReplicas: 2
    maxReplicas: 4

mysql:
  persistence:
    size: 20Gi
```

The Gateway is also enabled for production.

---

## 2. Environment Comparison

| Setting          | Dev      | Staging  | Production |
| ---------------- | -------- | -------- | ---------- |
| BankApp replicas | 1        | 2-3      | 2-4        |
| Image            | latest   | v1.2.0   | v1.2.0     |
| MySQL storage    | 2Gi      | 5Gi      | 20Gi       |
| HPA              | Disabled | Enabled  | Enabled    |
| Gateway          | Disabled | Disabled | Enabled    |

The important point is that the Kubernetes templates stay the same.

Only the values change.

---

## 3. Helm Hooks

I also learned about Helm hooks.

I added a `pre-install` and `pre-upgrade` hook to check whether MySQL is ready.

The hook uses:

```yaml
annotations:
  "helm.sh/hook": pre-install,pre-upgrade
```

The Job waits for MySQL:

```text
BankApp Helm deployment
        |
        v
Check MySQL
        |
        v
MySQL ready
        |
        v
Continue deployment
```

The hook also uses:

```yaml
"helm.sh/hook-delete-policy": before-hook-creation
```

This allows the previous hook Job to be removed before a new one is created.

---

## 4. Helm Test

I also learned that Helm can run tests against a deployed release.

The test uses a Kubernetes Pod with:

```yaml
"helm.sh/hook": test
```

The test checks the Spring Boot health endpoint:

```text
/actuator/health
```

The command to run it is:

```bash
helm test bankapp-dev -n dev
```

This gives a simple way to verify that the deployed application is responding.

---

## 5. Packaging a Helm Chart

A Helm chart can also be packaged into a `.tgz` file.

First I validate the chart:

```bash
helm lint bankapp/
```

Then package it:

```bash
helm package bankapp/
```

This creates a package similar to:

```text
bankapp-0.1.0.tgz
```

After changing the chart version:

```yaml
version: 0.2.0
```

I can package it again:

```bash
helm package bankapp/
```

The chart can then be distributed and installed from the package.

---

## 6. Helm and GitOps

I also looked at how Helm can fit into the AI-BankApp GitOps workflow.

### Raw Kubernetes approach

```text
Developer
   |
   v
GitHub Actions
   |
   v
Build Docker image
   |
   v
Update Kubernetes YAML
   |
   v
Git commit
   |
   v
ArgoCD
   |
   v
EKS
```

### Helm approach

```text
Developer
   |
   v
GitHub Actions
   |
   v
Build Docker image
   |
   v
Update Helm values
   |
   v
Git commit
   |
   v
ArgoCD
   |
   v
Helm Chart
   |
   v
EKS
```

Instead of changing a hardcoded image in a Kubernetes Deployment, the pipeline can update:

```yaml
bankapp:
  image:
    tag: <git-sha>
```

This makes the deployment configuration easier to manage.

---

## 7. Helm in ArgoCD

ArgoCD supports Helm charts directly.

Instead of pointing ArgoCD to:

```yaml
source:
  path: k8s
```

the application can point to:

```yaml
source:
  path: helm-chart/bankapp
  helm:
    valueFiles:
      - values-prod.yaml
```

ArgoCD can render the Helm chart and synchronize the generated Kubernetes resources.

---

## 8. Useful Helm Command for CI/CD

One command I learned today is:

```bash
helm upgrade --install bankapp bankapp/ \
  -f bankapp/values-prod.yaml \
  --set bankapp.image.tag=$GIT_SHA \
  -n bankapp \
  --create-namespace \
  --wait \
  --timeout 300s \
  --atomic
```

Important options:

### `--install`

Install the release if it doesn't already exist.

### `--wait`

Wait for Kubernetes resources to become ready.

### `--atomic`

If the deployment fails, Helm automatically rolls back the release.

This is useful for CI/CD because a failed deployment shouldn't leave the application in a partially upgraded state.

---

## 9. Helm vs Raw Manifests vs Kustomize

| Approach            | Best Use                                  |
| ------------------- | ----------------------------------------- |
| Raw Kubernetes YAML | Simple deployments                        |
| Helm                | Reusable charts and multiple environments |
| Kustomize           | Environment overlays and patches          |

For the AI-BankApp, Helm makes sense because the application has multiple components and different environment requirements.

---

## 10. Production Secret Management

One important thing I learned is that real production passwords should not be stored directly inside:

```text
values.yaml
```

For learning and local development, simple values are okay.

For production, better options include:

* AWS Secrets Manager + External Secrets Operator
* Sealed Secrets
* HashiCorp Vault

This is something I would improve before considering the chart production-ready.

---

## 11. My 3-Day Helm Journey

| Day    | What I Learned                                        | AI-BankApp Connection                                  |
| ------ | ----------------------------------------------------- | ------------------------------------------------------ |
| Day 78 | Helm basics, repositories, values, releases, rollback | Deployed MySQL using a community Helm chart            |
| Day 79 | Custom Helm charts and Go templates                   | Converted Kubernetes manifests into a custom chart     |
| Day 80 | Multiple environments, hooks, packaging and CI/CD     | Learned how the chart can be used in a GitOps workflow |

---

## Key Takeaway

The biggest thing I learned from these three days is that Helm is not just another way to write Kubernetes YAML.

It gives me a way to package Kubernetes resources and change their configuration without creating completely different manifests for every environment.

```text
One Chart
   +
Different Values
   =
Different Environments
```

Still learning, but I now have a much better understanding of how Helm can be used with Kubernetes and GitOps.
