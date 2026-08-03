# Lab 38 — Deploy Application & Infrastructure from Azure DevOps

> [!IMPORTANT]
> This lab demonstrates how to extend an Azure DevOps YAML pipeline with a production deployment stage. You will configure Azure authentication, deploy an application to Azure App Service, and implement approval-based production releases.

---

# Objective

Use Azure DevOps Release Pipelines / YAML Continuous Deployment (CD) to:

* 🔐 Configure Azure Service Connections.
* 🚀 Deploy applications to Azure App Service.
* 🏗️ Support infrastructure deployment workflows.
* ✅ Add production approval gates.
* 🔒 Apply deployment security best practices.

---

# Prerequisites

Before starting, ensure:

* ✅ Azure DevOps project created.
* ✅ Azure subscription available.
* ✅ Azure App Service created.
* ✅ CI pipeline from previous lab completed.
* ✅ Service connection permissions available.

---

# Deployment Workflow Overview

```text id="7p3m8q"
Developer Commit
        |
        ↓
Azure Repos
        |
        ↓
Build Pipeline
        |
        ↓
Deploy Dev Environment
        |
        ↓
Approval Gate
        |
        ↓
Deploy Production
        |
        ↓
Azure App Service
```

---

# Part 1 — Create Azure Service Connection

## Step 1 — Configure Service Connection

Navigate:

```text id="4m8q2v"
Project Settings
        ↓
Service Connections
        ↓
New Service Connection
        ↓
Azure Resource Manager
```

Select:

```text id="9q5m3v"
Service Principal
```

Purpose:

* Allows Azure DevOps to authenticate with Azure.
* Enables automated deployments.
* Provides controlled access to Azure resources.

---

# Part 2 — Add Production Deployment Stage

## Step 2 — Update `azure-pipelines.yml`

Add the following deployment stage:

```yaml id="5v8m2q"
- stage: Deploy_Prod

  displayName: 'Deploy to Production'


  dependsOn: Deploy_Dev


  jobs:

  - deployment: DeployProd

    environment: 'production'


    pool:

      vmImage: 'ubuntu-latest'


    strategy:

      runOnce:

        deploy:

          steps:

          - task: AzureWebApp@1

            inputs:

              azureSubscription: 'MyAzureServiceConnection'

              appType: 'webAppLinux'

              appName: 'my-devops-webapp'

              package: '$(Pipeline.Workspace)/drop/**/*.zip'

              runtimeStack: 'NODE|18-lts'
```

---

# Deployment Stage Explanation

## Deployment Stage

```yaml
stage: Deploy_Prod
```

Purpose:

* Defines production deployment workflow.
* Runs after successful development deployment.

---

## Dependency Control

```yaml
dependsOn: Deploy_Dev
```

Ensures:

```text
Build
 ↓
Deploy Dev
 ↓
Deploy Production
```

Production deployment only starts after Dev deployment succeeds.

---

## Production Environment

```yaml
environment: 'production'
```

Provides:

* Approval controls.
* Deployment history.
* Environment security.

---

## Azure Web App Deployment Task

Task:

```yaml
AzureWebApp@1
```

Purpose:

* Deploys application package.
* Connects Azure DevOps with Azure App Service.

Configuration:

| Setting          | Value                    |
| ---------------- | ------------------------ |
| Subscription     | MyAzureServiceConnection |
| Application Type | Linux Web App            |
| App Name         | my-devops-webapp         |
| Runtime          | NODE 18 LTS              |

---

# Part 3 — Configure Production Approval

## Step 3 — Add Approval Gate

Navigate:

```text id="6m3q9v"
Azure DevOps
        ↓
Environments
        ↓
production
        ↓
Approvals
```

Add:

```text id="2q8m5v"
Approvers
```

Purpose:

* Prevents unauthorized production releases.
* Enables controlled deployment workflow.

---

# Part 4 — Execute Pipeline

## Step 4 — Run Pipeline

Start pipeline execution.

Expected behavior:

```text id="8v4m2q"
Build Stage
      |
      ↓
Deploy Dev
      |
      ↓
Production Approval Required
      |
      ↓
Deploy Production
```

The pipeline pauses at:

```text id="5m7q3v"
Deploy_Prod
```

until approval is granted.

---

# Deployment Validation

After approval:

Verify:

* ✅ Production deployment completed.
* ✅ Azure App Service updated.
* ✅ Application is accessible.
* ✅ Deployment logs show success.

---

# Best Practice Tips

> [!TIP]
> Apply these practices for secure enterprise deployments.

---

## 🔐 Use Separate Service Principals Per Environment

Recommended:

```text id="3q7m9v"
Development Service Principal

Staging Service Principal

Production Service Principal
```

Benefits:

* Least privilege access.
* Better auditing.
* Reduced security risk.

---

## 🔵🟢 Use App Service Deployment Slots

Recommended workflow:

```text id="6x2m8v"
Production
    |
    |
Blue Slot ← New Version
    |
    |
Validation
    |
    ↓
Swap Slots
```

Benefits:

* Blue-green deployments.
* Minimal downtime.
* Safer releases.

---

## 🔑 Store Secrets in Azure Key Vault

Avoid:

```yaml
password: MySecret123
```

inside pipeline files.

Use:

```text id="9m5q2v"
Azure Key Vault
        ↓
Pipeline Secret Injection
        ↓
Application Deployment
```

Benefits:

* Secure credential management.
* Centralized secrets.
* Better compliance.

---

## 🧹 Configure Pipeline Retention Policies

Recommended:

```text id="4v8m3q"
Keep last 30 builds

Automatically delete older builds
```

Benefits:

* Reduces storage usage.
* Keeps pipeline history manageable.

---

# Validation Checklist

Verify:

* ✅ Azure Resource Manager service connection created.
* ✅ Production deployment stage added.
* ✅ Azure App Service task configured.
* ✅ Production environment created.
* ✅ Approval gate configured.
* ✅ Pipeline pauses for approval.
* ✅ Production deployment completes successfully.

---

# Summary

In this lab, you:

* Created an Azure service connection.
* Added a production deployment stage.
* Configured Azure App Service deployment.
* Implemented production approval controls.
* Learned secure Azure DevOps CD practices.

This workflow forms the foundation of enterprise-grade CI/CD pipelines with controlled application delivery and secure cloud deployments.
