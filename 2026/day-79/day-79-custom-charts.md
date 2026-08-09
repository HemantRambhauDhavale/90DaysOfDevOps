# Day 79 - Creating a Custom Helm Chart for AI-BankApp

## Goal

Today I created my own Helm Chart for the AI-BankApp.

Yesterday I used a community Helm Chart to deploy MySQL. Today I learned how to create a Helm Chart from existing Kubernetes YAML files.

The AI-BankApp had multiple Kubernetes manifests inside the `k8s/` directory.

The goal was to convert those raw manifests into a reusable and configurable Helm Chart.

---

# What I Learned

* How to create a Helm Chart
* `Chart.yaml`
* `values.yaml`
* Helm templates
* Helm template syntax
* `{{ .Values }}`
* `if` conditions
* `include`
* `toYaml`
* `nindent`
* `b64enc`
* Helm labels and helpers
* `helm lint`
* `helm template`
* Helm dry run
* Deploying a custom Helm Chart
* Using values to enable/disable application components

---

# Step 1 - Study the Existing Kubernetes Manifests

The AI-BankApp already had Kubernetes manifests inside:

```text
k8s/
```

Some of the important files were:

```text
namespace.yml
configmap.yml
secrets.yml
pv.yml
pvc.yml
bankapp-deployment.yml
mysql-deployment.yml
ollama-deployment.yml
service.yml
hpa.yml
gateway.yml
cert-manager.yml
```

These files were responsible for different parts of the application.

For example:

* `bankapp-deployment.yml` → Spring Boot application
* `mysql-deployment.yml` → MySQL
* `ollama-deployment.yml` → Ollama AI service
* `service.yml` → Kubernetes Services
* `configmap.yml` → Application configuration
* `secrets.yml` → Database credentials
* `pvc.yml` → Persistent storage
* `hpa.yml` → Horizontal Pod Autoscaler

---

# Step 2 - Create a Helm Chart

I created a separate directory:

```bash
mkdir helm-chart
cd helm-chart
```

Then I created a Helm Chart:

```bash
helm create bankapp
```

Helm generated the basic Chart structure automatically.

I removed the default template files because I wanted to create templates based on the AI-BankApp's existing Kubernetes manifests.

```bash
rm -rf bankapp/templates/*.yaml bankapp/templates/tests/
```

I kept:

```text
_helpers.tpl
NOTES.txt
```

because they can be customized later.

---

# Step 3 - Chart.yaml

I configured the chart metadata in:

```text
bankapp/Chart.yaml
```

Important fields include:

```yaml
apiVersion: v2
name: bankapp
description: AI-BankApp - Spring Boot banking application with MySQL and Ollama AI chatbot
type: application
version: 0.1.0
appVersion: "1.0.0"
```

## Important Difference

### version

This is the version of the Helm Chart.

### appVersion

This represents the version of the application packaged by the chart.

They are not the same thing.

---

# Step 4 - values.yaml

The most important idea I learned today was using `values.yaml`.

Instead of hardcoding configuration inside Kubernetes YAML, I moved configuration into:

```text
bankapp/values.yaml
```

For example:

```yaml
bankapp:
  replicaCount: 4

  image:
    repository: trainwithshubham/ai-bankapp-eks
    tag: "latest"

  service:
    type: ClusterIP
    port: 8080
```

MySQL configuration:

```yaml
mysql:
  enabled: true

  image:
    repository: mysql
    tag: "8.0"

  persistence:
    size: 5Gi
```

Ollama configuration:

```yaml
ollama:
  enabled: true
  model: tinyllama

  persistence:
    size: 10Gi
```

This means I can change configuration without modifying the template itself.

---

# Step 5 - ConfigMap Template

I converted the original ConfigMap into:

```text
templates/configmap.yaml
```

Instead of hardcoding values, I used Helm expressions.

Example:

```yaml
MYSQL_DATABASE: {{ .Values.config.mysqlDatabase | quote }}
```

Now the value comes from:

```yaml
config:
  mysqlDatabase: bankappdb
```

I also used:

```yaml
{{ include "bankapp.fullname" . }}
```

to generate resource names dynamically.

---

# Step 6 - Secret Template

I created:

```text
templates/secrets.yaml
```

One useful Helm function I learned was:

```text
b64enc
```

Instead of manually converting passwords into Base64, Helm can do it:

```yaml
MYSQL_ROOT_PASSWORD: {{ .Values.secrets.mysqlRootPassword | b64enc | quote }}
```

This makes the template easier to maintain.

---

# Step 7 - Storage Templates

I created:

```text
templates/storage.yaml
```

This template contains:

* StorageClass
* MySQL PVC
* Ollama PVC

I used conditions such as:

```yaml
{{- if .Values.mysql.enabled }}
```

This means the MySQL resources are created only when MySQL is enabled.

The same idea is used for Ollama.

---

# Step 8 - BankApp Deployment

I converted the Spring Boot Deployment into:

```text
templates/bankapp-deployment.yaml
```

The image is now configurable:

```yaml
image: "{{ .Values.bankapp.image.repository }}:{{ .Values.bankapp.image.tag }}"
```

Instead of changing the Deployment YAML every time, I can change:

```yaml
bankapp:
  image:
    tag: "latest"
```

or provide another value during installation.

---

# Step 9 - Init Containers

The original application waits for MySQL and Ollama.

I kept this behaviour in the Helm template.

For example:

```yaml
initContainers:
  - name: wait-for-mysql
```

The service name is generated dynamically using Helm:

```yaml
{{ include "bankapp.fullname" . }}-mysql
```

I also made the Ollama init container conditional:

```yaml
{{- if .Values.ollama.enabled }}
```

So when Ollama is disabled, that init container is not created.

---

# Step 10 - MySQL Deployment

I created:

```text
templates/mysql-deployment.yaml
```

The MySQL image comes from:

```yaml
.Values.mysql.image.repository
```

and:

```yaml
.Values.mysql.image.tag
```

The database password comes from the Kubernetes Secret.

The database storage comes from the PVC.

---

# Step 11 - Ollama Deployment

I created:

```text
templates/ollama-deployment.yaml
```

One useful improvement is that the Ollama model is configurable.

Instead of writing:

```text
tinyllama
```

directly inside the template, I use:

```yaml
{{ .Values.ollama.model }}
```

Now I can change the model through `values.yaml`.

---

# Step 12 - Services

I created:

```text
templates/services.yaml
```

It contains Services for:

* MySQL
* Ollama
* BankApp

The BankApp service uses:

```yaml
.Values.bankapp.service.type
```

and:

```yaml
.Values.bankapp.service.port
```

---

# Step 13 - HPA

I created:

```text
templates/hpa.yaml
```

The HPA is created only when:

```yaml
bankapp:
  autoscaling:
    enabled: true
```

The HPA configuration includes:

```yaml
minReplicas: 2
maxReplicas: 4
targetCPUUtilization: 70
```

This allows Kubernetes to automatically adjust BankApp replicas based on CPU utilization.

---

# Step 14 - Helm Template Functions I Learned

## `.Values`

Used to read configuration from `values.yaml`.

Example:

```yaml
{{ .Values.ollama.model }}
```

---

## `if`

Used for conditional resources.

Example:

```yaml
{{- if .Values.ollama.enabled }}
```

---

## `include`

Used to call a Helm helper.

Example:

```yaml
{{ include "bankapp.fullname" . }}
```

---

## `toYaml`

Converts a value into YAML.

Example:

```yaml
{{- toYaml .Values.bankapp.resources | nindent 12 }}
```

---

## `nindent`

Adds indentation to generated YAML.

This is important because Kubernetes YAML is indentation-sensitive.

---

## `b64enc`

Converts a string into Base64.

Example:

```yaml
{{ .Values.secrets.mysqlPassword | b64enc }}
```

---

# Step 15 - Validate the Chart

Before deploying, I used:

```bash
helm lint bankapp/
```

This checks the Helm Chart for common problems.

---

# Step 16 - Render the Templates

I used:

```bash
helm template my-bankapp bankapp/
```

This renders the Helm templates into normal Kubernetes YAML.

This helped me understand what Helm actually generates before sending anything to Kubernetes.

---

# Step 17 - Test with Different Values

I also tested how changing values affects the generated resources.

For example:

```bash
helm template my-bankapp bankapp/ \
  --set bankapp.image.tag=abc1234 \
  --set bankapp.replicaCount=2 \
  --set ollama.enabled=false
```

The important thing I learned here is that one value can control an entire component.

For example:

```text
ollama.enabled=false
```

can remove:

* Ollama Deployment
* Ollama Service
* Ollama PVC
* Ollama init container

from the rendered chart.

---

# Raw Kubernetes vs Helm

## Before Helm

The application needed multiple YAML files:

```text
k8s/
├── bankapp-deployment.yml
├── mysql-deployment.yml
├── ollama-deployment.yml
├── service.yml
├── configmap.yml
├── secrets.yml
├── pvc.yml
├── pv.yml
├── hpa.yml
└── ...
```

Each file had hardcoded configuration.

---

## With Helm

The application can be packaged as:

```text
bankapp/
├── Chart.yaml
├── values.yaml
└── templates/
    ├── configmap.yaml
    ├── secrets.yaml
    ├── storage.yaml
    ├── bankapp-deployment.yaml
    ├── mysql-deployment.yaml
    ├── ollama-deployment.yaml
    ├── services.yaml
    └── hpa.yaml
```

Now configuration is separated from the templates.

---

# Biggest Learning

The biggest thing I understood today is that Helm is not just about reducing the number of YAML files.

The main benefit is **reusability and configuration**.

The same chart can be used with different values.

For example:

```text
Development
    ↓
values-dev.yaml

Staging
    ↓
values-staging.yaml

Production
    ↓
values-prod.yaml
```

The templates can remain the same.

---

# Key Takeaways

Today I learned:

1. How to create a custom Helm Chart.
2. How `Chart.yaml` describes a chart.
3. How `values.yaml` stores configurable values.
4. How Helm templates generate Kubernetes manifests.
5. How to use conditions with `if`.
6. How to use Helm helper functions.
7. How `b64enc` can encode Secret values.
8. How `toYaml` and `nindent` help generate valid YAML.
9. How `helm lint` validates a chart.
10. How `helm template` helps debug before deployment.

---

# Day 79 Status

```text
 Custom Helm Chart created
 Chart.yaml configured
 values.yaml created
 BankApp template created
 MySQL template created
 Ollama template created
 Services template created
 ConfigMap template created
 Secret template created
 Storage template created
 HPA template created
 Helm lint practiced
 Helm template practiced
```

---

## Final Thought

Yesterday I learned how to use someone else's Helm Chart.

Today I learned how to create my own.

That was a big step for me because I'm starting to understand how Helm can be used in a real DevOps project instead of just learning commands.

**Day 79 completed. 🚀**
