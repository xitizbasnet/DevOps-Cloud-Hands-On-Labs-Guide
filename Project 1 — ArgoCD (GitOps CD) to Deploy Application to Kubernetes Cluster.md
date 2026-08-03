# Section 11 — Capstone Projects

## 🎯 Overview

These capstone projects combine multiple technologies and practices covered throughout the previous labs.

Each project is designed as a **portfolio-worthy DevOps deliverable** demonstrating practical implementation of:

* Cloud infrastructure
* Automation
* CI/CD
* Container orchestration
* Infrastructure as Code
* Monitoring
* GitOps practices

---

# 🚀 Project 1 — ArgoCD (GitOps CD) to Deploy Application to Kubernetes Cluster

## 📌 Objective

Implement a GitOps-based continuous deployment workflow using:

* Kubernetes
* ArgoCD
* GitHub
* Helm

The project demonstrates how application deployment can be automated by maintaining the desired Kubernetes state inside Git.

---

# 🛠️ Tools Used

| Tool          | Purpose                                     |
| ------------- | ------------------------------------------- |
| ☸️ Kubernetes | Container orchestration platform            |
| 🔄 ArgoCD     | GitOps continuous delivery controller       |
| 📦 Helm       | Kubernetes application packaging            |
| 🐙 GitHub     | Source repository and desired state storage |

---

# 🏗️ Architecture

```text
GitHub Repository
        |
        |  Desired Kubernetes State
        ↓
     ArgoCD
        |
        |  Watches Repository
        ↓
Auto-Sync Deployment
        |
        ↓
Kubernetes Cluster
```

## GitOps Workflow

1. Developer pushes Kubernetes manifests or Helm changes to GitHub.
2. ArgoCD continuously monitors the repository.
3. ArgoCD compares:

   * Desired state (Git repository)
   * Current state (Kubernetes cluster)
4. ArgoCD automatically synchronizes changes.
5. Kubernetes runs the updated application.

---

# 🚀 Implementation Steps

## Step 1 — Install ArgoCD

Create ArgoCD namespace:

```bash
kubectl create namespace argocd
```

Install ArgoCD:

```bash
kubectl apply -n argocd -f \
https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Verify ArgoCD pods:

```bash
kubectl get pods -n argocd
```

Expected components:

```text
argocd-server
argocd-repo-server
argocd-application-controller
argocd-dex-server
```

---

# Step 2 — Retrieve ArgoCD Admin Password

Retrieve the initial administrator password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret \
-o jsonpath="{.data.password}" | base64 -d
```

Store this password securely.

---

# Step 3 — Access ArgoCD Web Interface

Port-forward the ArgoCD server:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Access:

```text
https://localhost:8080
```

Login:

```text
Username:
admin

Password:
<retrieved-password>
```

---

# Step 4 — Install ArgoCD CLI

Download ArgoCD CLI:

```bash
curl -sSL -o argocd \
https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
```

Make executable:

```bash
chmod +x argocd
```

Move binary:

```bash
sudo mv argocd /usr/local/bin/
```

Verify installation:

```bash
argocd version
```

---

# Step 5 — Login Using ArgoCD CLI

Login:

```bash
argocd login localhost:8080 \
--username admin \
--insecure
```

---

# Step 6 — Create ArgoCD Application

Create application configuration:

```yaml
# argocd-app.yaml

apiVersion: argoproj.io/v1alpha1
kind: Application

metadata:
  name: devops-app
  namespace: argocd

spec:
  project: default

  source:
    repoURL: https://github.com/YOUR_USERNAME/DevOps-Labs.git
    targetRevision: main
    path: k8s/

  destination:
    server: https://kubernetes.default.svc
    namespace: production

  syncPolicy:
    automated:
      prune: true
      selfHeal: true

    syncOptions:
      - CreateNamespace=true
```

---

# Step 7 — Deploy ArgoCD Application

Apply application configuration:

```bash
kubectl apply -f argocd-app.yaml
```

Check application status:

```bash
argocd app get devops-app
```

Synchronize application:

```bash
argocd app sync devops-app
```

Verify deployment:

```bash
kubectl get pods -n production
```

---

# 💡 Best Practice Tips

> 🔐 **Follow GitOps Principles**

Git should be the single source of truth.

Recommended practices:

* Store Kubernetes manifests in Git.
* Avoid manual `kubectl apply` deployments in production.
* Review all infrastructure changes through pull requests.

---

> 📦 **Use App of Apps Pattern**

For large environments:

* Manage multiple applications from a single ArgoCD application.
* Standardize deployment structures.
* Simplify multi-team Kubernetes management.

---

> 🔑 **Configure SSO Authentication**

For team environments:

* Integrate ArgoCD with GitHub OAuth.
* Use enterprise identity providers.
* Avoid shared administrator accounts.

---

> 🔄 **Automate Container Image Updates**

Integrate:

* ArgoCD Image Updater
* Container registries such as Amazon ECR

Workflow:

```text
New Image Published
        ↓
Image Updater Detects Change
        ↓
Git Tag Updated
        ↓
ArgoCD Deploys New Version
```

---

# ✅ Project Completion Checklist

* [ ] Kubernetes cluster available.
* [ ] ArgoCD installed successfully.
* [ ] ArgoCD UI accessed.
* [ ] ArgoCD CLI configured.
* [ ] GitHub repository connected.
* [ ] Application manifest created.
* [ ] Automated synchronization enabled.
* [ ] Application deployed successfully to Kubernetes.

---

## 🎓 Skills Demonstrated

By completing this project, you demonstrate:

* GitOps deployment methodology
* Kubernetes application management
* Continuous delivery automation
* Declarative infrastructure management
* Production-style DevOps workflow
