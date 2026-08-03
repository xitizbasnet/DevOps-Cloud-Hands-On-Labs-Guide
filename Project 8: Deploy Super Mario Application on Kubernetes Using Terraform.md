# Project 8: Deploy Super Mario Application on Kubernetes Using Terraform

## Overview

This project demonstrates the deployment of a **containerized Super Mario application** on **Amazon Elastic Kubernetes Service (EKS)** using **Terraform**, **kubectl**, and **Helm**.

The implementation provisions a Kubernetes cluster on AWS using Terraform and deploys the application using Kubernetes manifests.

The solution demonstrates an end-to-end cloud-native deployment workflow:

* Infrastructure provisioning with Terraform.
* Managed Kubernetes cluster deployment using Amazon EKS.
* Application deployment using Kubernetes.
* External application exposure using Kubernetes LoadBalancer service.

---

# 🛠️ Tools and Technologies

| Tool / Technology                          | Purpose                                             |
| ------------------------------------------ | --------------------------------------------------- |
| 🏗️ Terraform                              | Automates AWS infrastructure provisioning           |
| ☸️ Amazon Elastic Kubernetes Service (EKS) | Provides managed Kubernetes cluster hosting         |
| ⚙️ kubectl                                 | Manages Kubernetes resources and deployments        |
| 📦 Helm                                    | Provides Kubernetes package management capabilities |

---

# 🏛️ Architecture Overview

The deployment architecture follows this workflow:

```text id="f5v8ax"
Terraform
    |
    ▼
AWS EKS Cluster
    |
    ▼
Managed Node Group
    |
    ▼
Kubernetes Deployment
    |
    ▼
Super Mario Container
    |
    ▼
Kubernetes LoadBalancer Service
    |
    ▼
External User Browser
```

---

# 🚀 Deployment Workflow

The deployment process includes:

1. Provisioning an Amazon EKS cluster using Terraform.
2. Creating managed worker nodes.
3. Configuring local Kubernetes access.
4. Deploying the Super Mario containerized application.
5. Creating a LoadBalancer service.
6. Accessing the application through the external IP address.

---

# 📁 Terraform EKS Cluster Configuration

## Create EKS Cluster Using Terraform Module

Terraform uses the community-maintained AWS EKS module to create the Kubernetes environment.

```hcl id="2q2p8s"
module "eks" {

  source  = "terraform-aws-modules/eks/aws"

  version = "~> 20.0"


  cluster_name = "super-mario-cluster"

  cluster_version = "1.28"


  vpc_id = module.vpc.vpc_id

  subnet_ids = module.vpc.private_subnets


  eks_managed_node_groups = {

    main = {

      instance_types = ["t3.medium"]

      min_size = 1

      max_size = 3

      desired_size = 2

    }

  }

}
```

---

# ⚙️ EKS Cluster Configuration Details

| Configuration      | Value                 |
| ------------------ | --------------------- |
| Cluster Name       | `super-mario-cluster` |
| Kubernetes Version | `1.28`                |
| Node Instance Type | `t3.medium`           |
| Minimum Nodes      | `1`                   |
| Maximum Nodes      | `3`                   |
| Desired Nodes      | `2`                   |
| Network            | Private Subnets       |

---

# 🔄 Deploy Infrastructure

## Initialize Terraform

```bash id="j8lq5m"
terraform init
```

---

## Review Infrastructure Changes

```bash id="wx3h8d"
terraform plan
```

> [!IMPORTANT]
> Review the Terraform plan before creating AWS infrastructure resources.

---

## Create EKS Infrastructure

```bash id="p9x2r4"
terraform apply
```

Terraform creates:

* EKS control plane.
* Managed node group.
* Required AWS networking resources.

---

# 🔑 Configure Kubernetes Access

After Terraform deployment, configure `kubectl` access.

```bash id="h9r1vq"
aws eks update-kubeconfig \
--name super-mario-cluster \
--region ap-south-1
```

Verify cluster connectivity:

```bash id="8c7mnb"
kubectl get nodes
```

Expected result:

```text id="6c1j8x"
NAME                  STATUS   ROLE
eks-node-group-node   Ready    <none>
```

---

# 🎮 Deploy Super Mario Application

## Kubernetes Deployment and Service

The Super Mario application is deployed as a Kubernetes Deployment with an external LoadBalancer service.

```yaml id="d0xj4v"
apiVersion: apps/v1
kind: Deployment

metadata:

  name: mario


spec:

  replicas: 1


  selector:

    matchLabels:

      app: mario


  template:

    metadata:

      labels:

        app: mario


    spec:

      containers:

      - name: mario

        image: sevenajay/mario:latest

        ports:

        - containerPort: 8080


---

apiVersion: v1

kind: Service

metadata:

  name: mario-svc


spec:

  type: LoadBalancer


  selector:

    app: mario


  ports:

  - port: 80

    targetPort: 8080
```

---

# 📦 Deploy Kubernetes Resources

Apply the Kubernetes configuration:

```bash id="7b5wqk"
kubectl apply -f mario.yaml
```

---

# 🌐 Access Super Mario Application

Retrieve the external service address:

```bash id="ax9r8w"
kubectl get svc mario-svc
```

Example output:

```text id="v9p7az"
NAME         TYPE           EXTERNAL-IP
mario-svc    LoadBalancer   xxx.xxx.xxx.xxx
```

Steps:

1. Copy the `EXTERNAL-IP` value.
2. Open the address in a web browser.
3. Access the deployed Super Mario application.

---

# 🔐 Kubernetes Deployment Architecture

Application flow:

```text id="m3x0kw"
User Browser
      |
      ▼
AWS Load Balancer
      |
      ▼
Kubernetes Service
      |
      ▼
Mario Deployment
      |
      ▼
Mario Container
```

---

# ✅ Best Practice Tips

## Use Terraform AWS EKS Module

Use the official community-supported EKS Terraform module.

Recommended module:

```text
terraform-aws-modules/eks/aws
```

Benefits:

* Battle-tested implementation.
* Community maintained.
* Reduces manual EKS configuration complexity.

---

## Enable Cluster Autoscaler

Configure Kubernetes Cluster Autoscaler after EKS creation.

Benefits:

* Automatically adjusts worker node count.
* Handles changing workloads.
* Improves resource utilization.

Example scaling model:

```text
Minimum Nodes: 1
Maximum Nodes: 3
```

---

## Destroy Lab Resources After Completion

Remove AWS resources after testing.

Command:

```bash id="v2f8ab"
terraform destroy
```

> [!WARNING]
> EKS clusters and supporting resources such as NAT Gateways can generate ongoing AWS costs. Destroy unused lab environments after completion.

---

## Use This Project for Portfolio Demonstration

This project demonstrates practical skills across:

* Terraform infrastructure automation.
* AWS EKS management.
* Kubernetes deployments.
* Container orchestration.
* Cloud-native application delivery.

---

# 📚 Summary

This project delivers a complete Kubernetes deployment workflow by combining:

* 🏗️ Terraform for AWS infrastructure automation.
* ☸️ Amazon EKS for managed Kubernetes.
* ⚙️ kubectl for application management.
* 📦 Helm-ready Kubernetes architecture.
* 🎮 Containerized Super Mario application deployment.

The implementation demonstrates an end-to-end DevOps workflow covering infrastructure provisioning, Kubernetes operations, and cloud-native application deployment.
