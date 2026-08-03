# Lab 45 — Deploy with AWS CodeDeploy

> [!IMPORTANT]
> This lab demonstrates how to deploy an application to an EC2 fleet using AWS CodeDeploy and an `appspec.yml` deployment configuration file.

> [!NOTE]
> AWS CodeDeploy automates application deployments to Amazon EC2 instances, allowing controlled releases, deployment hooks, validation checks, and rollback capabilities.

---

# Objective

Configure AWS CodeDeploy to:

* 🚀 Deploy applications automatically to EC2 instances.
* 📄 Define deployment instructions using `appspec.yml`.
* ⚙️ Execute lifecycle hooks during deployment.
* 🔍 Validate application health after deployment.
* 🔄 Support rollback and production deployment strategies.

---

# Prerequisites

Before starting, ensure:

* ✅ EC2 target instances are running.
* ✅ EC2 instances have network access to AWS services.
* ✅ CodeDeploy IAM service role created.
* ✅ Application artifacts available in Amazon S3.
* ✅ Deployment scripts prepared.
* ✅ EC2 IAM role configured for CodeDeploy access.

---

# AWS CodeDeploy Workflow Overview

```text
Developer Push
      |
      ↓
AWS CodePipeline
      |
      ↓
Amazon S3 Artifact
      |
      ↓
AWS CodeDeploy
      |
      ↓
EC2 Instances
      |
      ├── BeforeInstall
      ├── Install Application
      ├── Start Application
      └── Validate Deployment
```

---

# Step 1 — Install CodeDeploy Agent on Target EC2

The CodeDeploy agent must be installed and running on every deployment target.

## Update System Packages

```bash
sudo apt update
```

---

## Install Required Packages

```bash
sudo apt install -y ruby wget
```

---

## Download CodeDeploy Installer

```bash
cd /tmp

wget https://aws-codedeploy-ap-south-1.s3.ap-south-1.amazonaws.com/latest/install
```

---

## Install CodeDeploy Agent

```bash
chmod +x ./install

sudo ./install auto
```

---

## Start and Enable Agent Service

```bash
sudo systemctl start codedeploy-agent

sudo systemctl enable codedeploy-agent
```

---

## Verify Agent Status

```bash
sudo systemctl status codedeploy-agent
```

Expected:

```text
CodeDeploy agent is running
```

---

# Step 2 — Create appspec.yml

Create `appspec.yml` in the repository root:

```yaml
# appspec.yml

version: 0.0

os: linux

files:
  - source: /
    destination: /var/www/app

permissions:
  - object: /var/www/app
    pattern: "**"
    owner: www-data
    group: www-data

hooks:

  BeforeInstall:
    - location: scripts/stop_server.sh
      timeout: 30

  AfterInstall:
    - location: scripts/install_deps.sh
      timeout: 120

  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 30

  ValidateService:
    - location: scripts/validate.sh
      timeout: 30
```

---

# appspec.yml Lifecycle Reference

| Lifecycle Hook     | Purpose                                        |
| ------------------ | ---------------------------------------------- |
| `BeforeInstall`    | Stops existing application services            |
| `AfterInstall`     | Installs dependencies and prepares application |
| `ApplicationStart` | Starts application services                    |
| `ValidateService`  | Performs health validation                     |

---

# Step 3 — Create CodeDeploy Application

Navigate:

```text
AWS Console
    ↓
CodeDeploy
    ↓
Create Application
```

Configure:

```text
Compute Platform:

EC2/On-Premises
```

Purpose:

* Defines the application deployment target.
* Enables EC2 deployment management.

---

# Step 4 — Create Deployment Group

Create a deployment group:

```text
CodeDeploy
    ↓
Application
    ↓
Create Deployment Group
```

Configure:

## Target Instances

Attach EC2 instances using:

```text
EC2 Tags
```

Example:

```text
Environment = Production
Application = DevOps-App
```

---

## Service Role

Attach:

```text
CodeDeploy Service Role
```

Required permissions:

* Access EC2 instances.
* Retrieve deployment artifacts.
* Manage deployment lifecycle.

---

# Step 5 — Create Deployment

Create a deployment:

```text
CodeDeploy
    ↓
Create Deployment
```

Specify:

```text
Revision Location:

Amazon S3 Artifact Location
```

CodeDeploy will:

```text
Download Artifact
        ↓
Execute appspec.yml
        ↓
Run Deployment Hooks
        ↓
Validate Application
        ↓
Complete Deployment
```

---

# Deployment Script Structure

Recommended repository layout:

```text
Application Repository
│
├── appspec.yml
│
├── scripts
│   ├── stop_server.sh
│   ├── install_deps.sh
│   ├── start_server.sh
│   └── validate.sh
│
└── Application Files
```

---

# Best Practice Tips

> [!TIP]
> Follow these recommendations for reliable production deployments.

---

## 🔵 Use Blue/Green Deployments

Recommended deployment strategy:

```text
Blue Environment
        |
        ↓
Deploy New Version
        |
        ↓
Green Environment
        |
        ↓
Traffic Switch
```

Benefits:

* Zero downtime deployment.
* Safer releases.
* Easy rollback.

---

## 🔍 Validate Application Health

The `ValidateService` hook is critical.

Example:

```bash
curl -f http://localhost:8080/health
```

Purpose:

* Confirms application availability.
* Prevents failed deployments from being marked successful.

---

## 🔐 Configure EC2 IAM Role

EC2 instances require permissions to:

```text
EC2 Instance
      |
      ↓
IAM Role
      |
      ↓
Amazon S3 Artifact Access
```

Avoid:

* Hardcoded AWS credentials.
* Manual artifact copying.

---

## 🔄 Enable Automatic Rollback

Configure:

```text
CodeDeploy
    ↓
Deployment Group
    ↓
Rollback Configuration
```

Rollback triggers:

* Deployment failure.
* Health check failure.
* Lifecycle hook failure.

---

# Validation Checklist

Verify:

* ✅ CodeDeploy agent installed on EC2.
* ✅ Agent service running.
* ✅ `appspec.yml` created correctly.
* ✅ Deployment scripts available.
* ✅ CodeDeploy application created.
* ✅ Deployment group configured.
* ✅ EC2 instances registered successfully.
* ✅ Deployment completed successfully.
* ✅ Application health validation passed.

---

# Summary

In this lab, you:

* Installed and configured the AWS CodeDeploy agent.
* Created an `appspec.yml` deployment definition.
* Configured EC2 deployment targets.
* Executed application deployment workflows.
* Applied production deployment best practices.

This completes the AWS CodePipeline → CodeBuild → CodeDeploy CI/CD deployment flow.
