# Kubernetes Demo – Single Node k3s Production-Style Deployment

## Overview

This project demonstrates how to set up a **single-node Kubernetes cluster (k3s)** on a VPS and deploy a production-style application using core Kubernetes primitives.

The goal is to showcase **real-world DevOps practices** such as configuration management, secrets handling, health checks, scaling, and self-healing without relying on managed cloud Kubernetes services.

---

## Architecture

- **Platform:** k3s (lightweight Kubernetes)
- **Environment:** Single VPS (Linux)
- **Namespace:** `prod`
- **Application:** Nginx (demo web service)
- **Exposure:** NodePort Service
- **Runtime:** containerd

### Resource Hierarchy

Namespace: prod
├── Deployment: demo-deploy
│   └── ReplicaSet
│       └── Pods
│           └── Container: nginx
├── Service: demo-svc (NodePort)
├── ConfigMap: demo-config
└── Secret: demo-secret


YAML

```
---

## Kubernetes Objects Used

- **Namespace** – environment isolation (`prod`)
- **Deployment** – application lifecycle, scaling, self-healing
- **ReplicaSet** – maintains desired Pod count
- **Pod** – runs the application container
- **Service (NodePort)** – exposes the application externally
- **ConfigMap** – non-sensitive configuration
- **Secret** – sensitive configuration (credentials)
- **Liveness & Readiness Probes** – health checking
- **Resource Requests & Limits** – basic resource management

---

## Prerequisites

- Linux VPS (Ubuntu 20.04 / 22.04 recommended)
- SSH access (root or sudo user)
- `curl` installed
- Basic Kubernetes and Git knowledge

---

## Installation – Kubernetes (k3s)

Install k3s on the VPS:

bash
curl -sfL https://get.k3s.io | sh -
```
Verify cluster status:
```
kubectl get nodes
```
## Deploy the Application
Apply Kubernetes manifests in order:
```
kubectl apply -f namespace.yaml
kubectl apply -f configmap.yaml
kubectl apply -f secret.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```
Verify resources:
```
kubectl get ns
kubectl get deploy -n prod
kubectl get pods -n prod
kubectl get svc -n prod
```
##Access the Application
From the VPS:
```
curl http://127.0.0.1:30081
```
From your local machine:
```
http://<VPS_IP>:30081
```
Ensure the VPS firewall or security group allows traffic on port 30081.

##Scaling & Self-Healing
Scale the application:
```
kubectl scale deploy demo-deploy -n prod --replicas=5
kubectl get pods -n prod
```

Test self-healing by deleting a Pod:
```
kubectl delete pod <POD_NAME> -n prod
kubectl get pods -n prod
```
Kubernetes will automatically create a new Pod to maintain the desired replica count.

## Configuration Management

### ConfigMap
Used for non-sensitive application configuration:

- Application name

- Environment variables

### Secret
Used for sensitive data:

- Credentials

- Passwords (base64-encoded)

Secrets are never baked into container images.

## Project Files

| File              | Description                               |
| ----------------- | ----------------------------------------- |
| `namespace.yaml`  | Defines the `prod` namespace              |
| `configmap.yaml`  | Application configuration                 |
| `secret.yaml`     | Sensitive configuration                   |
| `deployment.yaml` | Application deployment, probes, resources |
| `service.yaml`    | NodePort service exposing the app         |
| `README.md`       | Project documentation                     |

## Why This Project Matters

This project demonstrates:

- Kubernetes cluster bootstrapping without managed services

- Production-style application deployment

- Separation of configuration and secrets

- Horizontal scaling and self-healing

- Practical DevOps workflows on real infrastructure

It serves as a strong foundation for moving towards:

- Multi-node clusters

- Ingress controllers

- HPA (Horizontal Pod Autoscaler)

- Persistent storage

- Managed Kubernetes platforms (EKS, GKE, AKS)
