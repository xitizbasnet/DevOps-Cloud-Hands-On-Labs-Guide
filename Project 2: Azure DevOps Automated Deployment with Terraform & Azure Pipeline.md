# Project 2: Azure DevOps Automated Deployment with Terraform & Azure Pipeline

## Overview

This project demonstrates an automated infrastructure deployment workflow using **Azure DevOps Pipelines**, **Terraform**, and **Azure App Service**.

The pipeline automates Terraform validation, planning, and deployment processes while following infrastructure-as-code (IaC) best practices.

---

## 🛠️ Tools and Technologies

| Tool / Technology         | Purpose                                                              |
| ------------------------- | -------------------------------------------------------------------- |
| 🔷 Azure DevOps Pipelines | Automates CI/CD workflows for infrastructure deployment              |
| 🏗️ Terraform             | Enables Infrastructure as Code (IaC) for Azure resource provisioning |
| ☁️ Azure App Service      | Hosts and manages web applications on Microsoft Azure                |

---

# 🚀 Deployment Workflow

The Azure DevOps pipeline performs the following actions:

1. Detects Terraform configuration changes.
2. Initializes Terraform with Azure remote state storage.
3. Creates a Terraform execution plan.
4. Requires approval before applying infrastructure changes.
5. Applies Terraform configuration to the Azure environment.

---

# 📁 Pipeline Configuration

## Terraform Plan and Apply Pipeline

**File:** `azure-pipelines-infra.yml`

The pipeline is triggered when changes are made inside the Terraform directory.

```yaml
trigger:
  paths:
    include:
      - 'terraform/**'

stages:

- stage: TerraformPlan

  jobs:

  - job: Plan

    steps:

    - task: TerraformInstaller@0
      inputs:
        terraformVersion: '1.5.7'

    - task: TerraformTaskV4@4
      inputs:
        provider: 'azurerm'
        command: 'init'
        backendServiceArm: 'AzureServiceConnection'
        backendAzureRmResourceGroupName: 'tf-state-rg'
        backendAzureRmStorageAccountName: 'tfstatestorage'
        backendAzureRmContainerName: 'tfstate'
        backendAzureRmKey: 'prod.terraform.tfstate'

    - task: TerraformTaskV4@4
      inputs:
        provider: 'azurerm'
        command: 'plan'
        environmentServiceNameAzureRM: 'AzureServiceConnection'


- stage: TerraformApply

  dependsOn: TerraformPlan

  jobs:

  - deployment:

    Apply

    environment: 'production'

    # Requires approval

    strategy:

      runOnce:

        deploy:

          steps:

          - task: TerraformTaskV4@4
            inputs:
              provider: 'azurerm'
              command: 'apply'
              environmentServiceNameAzureRM: 'AzureServiceConnection'
```

---

# 🔄 Pipeline Stages Explained

## Stage 1: Terraform Plan

### Purpose

Creates a preview of infrastructure changes before deployment.

### Actions Performed

* Installs the required Terraform version.
* Initializes Terraform.
* Connects Terraform to Azure remote state storage.
* Generates an execution plan.

---

## Stage 2: Terraform Apply

### Purpose

Applies approved Terraform changes to the Azure environment.

### Actions Performed

* Runs only after the Terraform Plan stage completes successfully.
* Deploys changes to the production environment.
* Requires approval before execution.

> [!IMPORTANT]
> Terraform apply should not run automatically in production environments without a review and approval process.

---

# 🔐 Terraform State Management

Terraform state is stored remotely using Azure Blob Storage.

Configured backend:

| Configuration   | Value                    |
| --------------- | ------------------------ |
| Resource Group  | `tf-state-rg`            |
| Storage Account | `tfstatestorage`         |
| Container       | `tfstate`                |
| State File      | `prod.terraform.tfstate` |

Remote state storage provides centralized state management and supports team collaboration.

---

# ✅ Best Practice Tips

## Store Terraform State in Azure Blob Storage

Azure Blob Storage should be used for Terraform state management.

Benefits:

* Enables team collaboration.
* Provides centralized state storage.
* Supports state locking mechanisms.

---

## Require Plan Review Before Apply

> [!WARNING]
> Never run `terraform apply` in a production pipeline without reviewing the Terraform plan and implementing an approval gate.

Recommended workflow:

1. Run Terraform plan.
2. Review proposed infrastructure changes.
3. Obtain approval.
4. Execute Terraform apply.

---

## Use Separate State Management per Environment

Recommended approaches:

* Use Terraform workspaces.
* Maintain separate Terraform state files for each environment.

Example environments:

```
dev.terraform.tfstate
test.terraform.tfstate
prod.terraform.tfstate
```

This prevents accidental changes between environments.

---

## Enable Terraform State Locking

> [!TIP]
> Lock Terraform state using Azure Storage Account leases to prevent concurrent deployments.

Benefits:

* Prevents multiple users or pipelines from modifying infrastructure simultaneously.
* Reduces the risk of state corruption.
* Improves deployment reliability.

---

# 📚 Summary

This Azure DevOps Terraform pipeline provides a secure and automated deployment process by combining:

* Infrastructure as Code using Terraform.
* Automated CI/CD using Azure DevOps Pipelines.
* Approval-based production deployments.
* Remote Terraform state management using Azure Blob Storage.

Following these practices improves deployment consistency, security, and collaboration across IT teams.
