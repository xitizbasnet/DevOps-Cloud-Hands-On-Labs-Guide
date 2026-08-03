# Lab 19 — Docker Registry

> [!IMPORTANT]
> This lab demonstrates how to store and distribute Docker images using container registries. You will push images to **Docker Hub** and **AWS Elastic Container Registry (ECR)** and learn registry security best practices.

## Objective

Push images to:

* 🐳 Docker Hub.
* ☁️ AWS ECR (Elastic Container Registry).

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 18 — Working with Docker Images**.
* ✅ Docker installed and running.
* ✅ A Docker Hub account.
* ✅ AWS CLI configured with appropriate permissions.
* ✅ An existing Docker image (`myapp:1.0`).

---

# Procedure

# Part 1 — Docker Hub Registry

## Step 1 — Login to Docker Hub

Authenticate with Docker Hub:

```bash id="4n8m2q"
docker login -u YOUR_DOCKERHUB_USERNAME
```

Enter your Docker Hub password or access token when prompted.

---

## Step 2 — Tag Docker Image

Tag the local image with your Docker Hub repository name:

```bash id="7p3m9x"
docker tag myapp:1.0 YOUR_USERNAME/myapp:1.0
```

Replace:

```text id="6q2m8v"
YOUR_USERNAME
```

with your Docker Hub username.

---

## Step 3 — Push Image to Docker Hub

Upload the image:

```bash id="9m5x3k"
docker push YOUR_USERNAME/myapp:1.0
```

The image is now available in your Docker Hub repository.

---

## Step 4 — Pull Image from Anywhere

Download the image from another system:

```bash id="2v8n6m"
docker pull YOUR_USERNAME/myapp:1.0
```

---

# Part 2 — AWS ECR (Elastic Container Registry)

## Step 1 — Create an ECR Repository

Using AWS CLI:

```bash id="5m7q2x"
aws ecr create-repository \
--repository-name myapp \
--region ap-south-1
```

This creates a private AWS container registry repository.

---

## Step 2 — Authenticate Docker with AWS ECR

Login Docker to ECR:

```bash id="8q4m6v"
aws ecr get-login-password --region ap-south-1 | \
docker login --username AWS \
--password-stdin 123456789012.dkr.ecr.ap-south-1.amazonaws.com
```

Replace:

```text id="1x9m5p"
123456789012
```

with your AWS account ID.

---

## Step 3 — Tag Image for ECR

Tag the Docker image:

```bash id="6p3v8m"
docker tag myapp:1.0 \
123456789012.dkr.ecr.ap-south-1.amazonaws.com/myapp:1.0
```

---

## Step 4 — Push Image to ECR

Upload the image:

```bash id="9k2m7x"
docker push 123456789012.dkr.ecr.ap-south-1.amazonaws.com/myapp:1.0
```

The image is now stored in AWS ECR.

---

# Container Registry Workflow

The image publishing workflow:

```text id="3m7q9v"
Developer Builds Image
          |
          ↓
Docker Image
          |
          ↓
Tag Image
          |
          ↓
Authenticate Registry
          |
          ↓
Push Image
          |
          ↓
Deploy Anywhere
```

---

# Registry Comparison

| Feature         | Docker Hub                     | AWS ECR             |
| --------------- | ------------------------------ | ------------------- |
| Provider        | Docker                         | AWS                 |
| Default Access  | Public/Private                 | Private             |
| AWS Integration | Limited                        | Native AWS services |
| Best For        | Open-source and general images | AWS workloads       |
| Authentication  | Docker credentials/token       | IAM credentials     |

---

# Best Practice Tips

> [!TIP]
> Follow registry security practices to protect application images and credentials.

## ☁️ Use ECR for AWS Workloads

For AWS-based applications:

* Prefer ECR for ECS/EKS deployments.
* Benefit from native AWS integration.
* Avoid unnecessary image transfer costs.

ECR is recommended for:

* Amazon ECS.
* Amazon EKS.
* AWS Lambda container images.

---

## 🔍 Enable ECR Image Scanning

Enable:

```text id="8v3m5q"
ECR Image Scanning on Push
```

Benefits:

* Automatically detects CVEs.
* Improves container security.
* Supports compliance requirements.

---

## 🧹 Configure ECR Lifecycle Policies

Implement lifecycle rules to:

* Automatically remove unused images.
* Reduce storage costs.
* Maintain registry hygiene.

Example policy:

```text id="5q7m9x"
Delete images older than 30 days
```

---

## 🔐 Protect Private Application Images

For proprietary applications:

Use:

* Private Docker Hub repositories.
* AWS ECR private repositories.

Never include:

* Passwords.
* API keys.
* Access tokens.
* Secrets.

inside Docker images.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Docker Hub authentication completed.
* ✅ Image tagged for Docker Hub.
* ✅ Image pushed to Docker Hub.
* ✅ Image pulled successfully.
* ✅ AWS ECR repository created.
* ✅ Docker authenticated with ECR.
* ✅ Image tagged for ECR.
* ✅ Image pushed successfully to ECR.

---

# Summary

In this lab, you:

* Published Docker images to Docker Hub.
* Created and configured an AWS ECR repository.
* Authenticated Docker with cloud registries.
* Tagged and pushed container images.
* Learned container registry security best practices.

You are now ready to continue with the next lab in the **Docker** learning path.
