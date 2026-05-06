# Day 50 – Kubernetes Architecture & Cluster Setup

Today I started learning Kubernetes.

## Why Kubernetes

- Container orchestration platform
- Manages containers at scale
- Handles scaling and self-healing

---

## Kubernetes Architecture

### Control Plane
- API Server
- etcd
- Scheduler
- Controller Manager

### Worker Node
- kubelet
- kube-proxy
- Container Runtime

---

## kubectl

- Installed kubectl CLI
- Used to communicate with cluster

Commands:
- kubectl get nodes
- kubectl get pods
- kubectl cluster-info

---

## Local Cluster

- Used kind for local Kubernetes cluster
- Verified cluster with kubectl

---

## Explored Cluster

- Nodes
- Namespaces
- kube-system pods

Important pods:
- kube-apiserver
- etcd
- kube-scheduler
- coredns

---

## kubeconfig

- Stores cluster connection details
- Default location:
  ~/.kube/config

---

## Summary

Today I learned:

- Kubernetes basics
- Cluster architecture
- Local cluster setup
- kubectl commands
- Kubernetes system components
