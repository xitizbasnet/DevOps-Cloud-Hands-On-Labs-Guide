# Project 3: Containerized Application Deployment on Azure Kubernetes Service (AKS)

## Overview

This project demonstrates the deployment of a **containerized application** on **Azure Kubernetes Service (AKS)** using **Docker**, **Azure Container Registry (ACR)**, and **Azure DevOps**.

The solution provides a cloud-native deployment architecture that enables scalable container orchestration, secure image management, monitoring, and environment isolation.

---

# 🛠️ Tools and Technologies

| Tool / Technology                 | Purpose                                                |
| --------------------------------- | ------------------------------------------------------ |
| 🐳 Docker                         | Builds and packages applications into container images |
| 📦 Azure Container Registry (ACR) | Stores and manages private container images in Azure   |
| ☸️ Azure Kubernetes Service (AKS) | Provides managed Kubernetes cluster orchestration      |
| 🔷 Azure DevOps                   | Supports CI/CD automation and deployment workflows     |

---

# 🚀 Deployment Workflow

The deployment process includes:

1. Creating an Azure resource group.
2. Creating an Azure Container Registry.
3. Creating an AKS cluster.
4. Integrating AKS with ACR for secure container image access.
5. Enabling Azure Monitor for container monitoring.
6. Connecting to the AKS cluster using Kubernetes tools.
7. Verifying cluster node availability.

---

# ⚙️ AKS Cluster Deployment Using Azure CLI

## Step 1: Create Azure Resource Group

Create a resource group to host Azure resources.

```bash
az group create \
  --name DevOps-RG \
  --location southindia
```

---

## Step 2: Create Azure Container Registry (ACR)

Create a private container registry for storing Docker images.

```bash
az acr create \
  --resource-group DevOps-RG \
  --name devopsacr \
  --sku Basic
```

---

## Step 3: Create AKS Cluster

Create an AKS cluster and connect it with Azure Container Registry.

```bash
az aks create \
  --resource-group DevOps-RG \
  --name devops-aks \
  --node-count 2 \
  --node-vm-size Standard_B2s \
  --attach-acr devopsacr \
  --enable-addons monitoring \
  --generate-ssh-keys
```

### Configuration Details

| Configuration      | Value                   |
| ------------------ | ----------------------- |
| Resource Group     | `DevOps-RG`             |
| AKS Cluster Name   | `devops-aks`            |
| Node Count         | `2`                     |
| Node VM Size       | `Standard_B2s`          |
| Container Registry | `devopsacr`             |
| Monitoring         | Enabled                 |
| SSH Keys           | Automatically generated |

---

## Step 4: Connect to AKS Cluster

Retrieve Kubernetes cluster credentials.

```bash
az aks get-credentials \
  --resource-group DevOps-RG \
  --name devops-aks
```

---

## Step 5: Verify AKS Nodes

Confirm that Kubernetes nodes are available.

```bash
kubectl get nodes
```

Expected result:

```text
NAME                           STATUS   ROLES   AGE
aks-nodepool1-xxxxx            Ready    agent   5m
aks-nodepool2-xxxxx            Ready    agent   5m
```

---

# 🔐 AKS and ACR Integration

AKS is connected directly with Azure Container Registry.

Benefits:

* No Docker registry secrets are required.
* Authentication between AKS and ACR is managed automatically.
* Container images can be securely pulled from ACR.

> [!TIP]
> Use AKS and ACR integration instead of manually creating Kubernetes image pull secrets whenever possible.

---

# 📊 Monitoring Configuration

Azure Monitor for Containers is enabled during AKS deployment.

Benefits:

* Collects container and Kubernetes logs.
* Provides cluster health visibility.
* Sends monitoring data to Azure Log Analytics workspace.
* Helps troubleshoot application and infrastructure issues.

> [!IMPORTANT]
> Enable monitoring for production AKS clusters to maintain operational visibility and faster incident response.

---

# ✅ Best Practice Tips

## Use Managed Node Pools

Use AKS managed node pools for improved cluster management.

Benefits:

* Built-in scaling capabilities.
* Simplified node lifecycle management.
* Better resource management.

---

## Integrate ACR with AKS

Use native ACR integration.

Benefits:

* Eliminates manual Docker registry secrets.
* Provides seamless authentication.
* Improves container image security.

---

## Enable Azure Monitor for Containers

Enable monitoring for all production workloads.

Benefits:

* Centralized logging.
* Performance monitoring.
* Faster troubleshooting.
* Integration with Azure Log Analytics.

---

## Use Kubernetes Namespaces for Environment Isolation

Use namespaces to separate different application environments within the same cluster.

Recommended structure:

```text
AKS Cluster

├── dev
├── staging
└── prod
```

Benefits:

* Improves workload isolation.
* Simplifies access control.
* Supports multiple deployment environments.

---

# 📚 Summary

This AKS deployment solution provides a scalable and secure container platform by combining:

* 🐳 Docker containerization.
* 📦 Azure Container Registry for private image storage.
* ☸️ Azure Kubernetes Service for container orchestration.
* 🔷 Azure DevOps for automation.
* 📊 Azure Monitor for operational visibility.

Following these practices enables reliable, production-ready Kubernetes deployments on Microsoft Azure.
