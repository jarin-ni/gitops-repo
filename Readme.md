# YourInfo – Argo CD GitOps Deployment

This repository contains an **enterprise-grade GitOps setup** using **Argo CD** to deploy the Docker image  
`smileshosting/yourinfo`  
to Kubernetes and expose it securely via **Traefik + Let’s Encrypt**.

---

## 🌐 Application Overview

| Item | Value |
|---|---|
| Application Name | yourinfo |
| Docker Image | smileshosting/yourinfo |
| Namespace | yourinfo |
| URL | https://yourinfo.smileshosting.com |
| GitOps Tool | Argo CD |
| Ingress | Traefik |
| TLS | cert-manager (Let’s Encrypt) |

---

## 🧠 Architecture Overview

```text
Argo CD (namespace: argocd)
└── Application: yourinfo
└── Namespace: yourinfo
├── Deployment (Docker image: smileshosting/yourinfo)
├── Service (ClusterIP)
└── Ingress (Traefik + TLS)

- Argo CD manages **all Kubernetes resources**
- Git is the **single source of truth**
- No manual `kubectl apply` in runtime namespaces

---

## 📁 Repository Structure

.
├── applications/
│ └── yourinfo.yaml # Argo CD Application
│
├── environments/
│ └── yourinfo/
│ ├── deployment.yaml # Kubernetes Deployment
│ ├── service.yaml # Kubernetes Service
│ └── ingress.yaml # Traefik Ingress + TLS
│
└── README.md


---

## 🔐 Argo CD Project

The application runs under the **`enterprise-apps`** Argo CD project.

### Project Capabilities
- Allows Applications in `argocd`
- Allows workloads only in `yourinfo`
- Allows Ingress, cert-manager, and required cluster resources
- Supports namespace auto-creation

---

## 🚀 Deployment Workflow (GitOps)

1. Push changes to this repository
2. Argo CD detects changes automatically
3. Kubernetes state is reconciled
4. Application is updated with **self-healing**

> 🚫 No direct kubectl changes in the `yourinfo` namespace

---

## 📦 Argo CD Application

- Auto-sync enabled
- Auto namespace creation enabled
- Self-heal and prune enabled

```yaml
syncPolicy:
  automated:
    prune: true
    selfHeal: true
  syncOptions:
  - CreateNamespace=true

🌍 Ingress & TLS

Ingress controller: Traefik

TLS issuer: Let’s Encrypt

Full site exposed at /

https://yourinfo.smileshosting.com


DNS must point to the Kubernetes public IP.

🔍 Verification Commands
kubectl get app -n argocd
kubectl get pods -n yourinfo
kubectl get svc -n yourinfo
kubectl get ingress -n yourinfo
kubectl get certificate -n yourinfo


Expected:

Application: Synced / Healthy

Pods: Running

Certificate: READY = True

🧪 Troubleshooting
Application not syncing
kubectl describe application yourinfo -n argocd

Ingress not reachable

Check DNS

Verify Traefik is running

Confirm port 80/443 are open
