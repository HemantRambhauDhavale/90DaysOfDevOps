# Day 54 – Kubernetes ConfigMaps & Secrets

## What I Learned

Today I learned how Kubernetes manages configuration and sensitive data using ConfigMaps and Secrets.

---

# ConfigMaps

ConfigMaps are used for non-sensitive configuration.

Created ConfigMaps:
- From literals
- From files

Commands used:

```bash id="b7n5x2"
kubectl create configmap app-config \
--from-literal=APP_ENV=production \
--from-literal=APP_DEBUG=false \
--from-literal=APP_PORT=8080
```

Inspect:

```bash id="v1r8k6"
kubectl describe configmap app-config
kubectl get configmap app-config -o yaml
```

---

# ConfigMap From File

Created:
- nginx custom config
- health endpoint

Command:

```bash id="t6m2q1"
kubectl create configmap nginx-config \
--from-file=default.conf=default.conf
```

---

# ConfigMap Usage

Used ConfigMaps:
- As environment variables
- As volume mounts

Learned:
- envFrom injects all keys
- Mounted files update automatically
- Environment variables do NOT auto-update

---

# Secrets

Created Secret:

```bash id="n4k8w0"
kubectl create secret generic db-credentials \
--from-literal=DB_USER=admin \
--from-literal=DB_PASSWORD=s3cureP@ssw0rd
```

Inspect:

```bash id="u2p9m4"
kubectl get secret db-credentials -o yaml
```

---

# Base64 Decoding

Decoded secret:

```bash id="r8x1k5"
echo '<base64-value>' | base64 --decode
```

Important learning:
- Base64 is encoding
- NOT encryption

---

# Secret Usage

Used Secrets:
- Environment variables
- Volume mounts

Mounted files contain plaintext values automatically.

---

# Live ConfigMap Update

Updated ConfigMap:

```bash id="f7m3q2"
kubectl patch configmap live-config \
--type merge \
-p '{"data":{"message":"world"}}'
```

Learned:
- Mounted ConfigMaps update automatically
- Environment variables require Pod restart

---

# Important Learning

## ConfigMaps
Used for non-sensitive configuration.

## Secrets
Used for sensitive data.

## Environment Variables
Loaded during Pod startup only.

## Volume Mounts
Support live updates.

---

# Summary

Today I learned:
- Kubernetes ConfigMaps
- Kubernetes Secrets
- Environment variable injection
- Volume mounts
- Base64 decoding
- Live configuration updates
- Secure configuration management
