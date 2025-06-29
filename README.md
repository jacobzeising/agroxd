---
description: 'PoC ArgoCD Setup: Provision a local Kubernetes cluster with ArgoCD and deploy a sample application.'
tools: [ 'argocd', 'kubernetes', 'kind' ]
---

# 🚀 ArgoCD Local PoC Setup

This guide walks you through setting up a local Kubernetes cluster using [kind](https://kind.sigs.k8s.io/) and installing [ArgoCD](https://argo-cd.readthedocs.io/) for GitOps-based application deployment.

---

## 1. Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [kind](https://kind.sigs.k8s.io/)

---

## 2. Install ArgoCD

Deploy ArgoCD to your Kubernetes cluster:

```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

To access the ArgoCD UI, forward the API server port:

```sh
kubectl port-forward -n argocd service/argocd-server 8443:443
```

---

## 3. Accessing the ArgoCD Dashboard

Retrieve the initial admin password:

```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
```

Open your browser and navigate to [https://localhost:8443](https://localhost:8443).  
Login with username: `admin` and the password you just retrieved.

---

### Creating a New Application in ArgoCD

1. Click **New App** in the ArgoCD UI.
2. Fill in the following details:
   - **Application Name**: `nginx-app`
   - **Project**: `default`
   - **Sync Policy**: `Manual`
   - **Repository URL**: `https://github.com/jacobzeising/agroxd`
   - **Revision**: `HEAD`
   - **Path**: `k8s`
   - **Cluster**: `https://kubernetes.default.svc`
   - **Namespace**: `default`
3. Click **Create** to finish.

You can now manage and sync your application directly from the ArgoCD dashboard.

