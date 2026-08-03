# Lab 32 — Use Terraform with AWS & Azure Cloud

> [!IMPORTANT]
> This lab demonstrates how to configure Terraform for **multi-cloud infrastructure management** using AWS and Azure providers. You will define required providers, configure cloud authentication, query cloud resources, and execute the Terraform workflow.

## Objective

Configure AWS and Azure providers and understand the Terraform workflow.

By the end of this lab, you will learn how to:

* ☁️ Configure AWS provider.
* ☁️ Configure Azure provider.
* 📦 Define Terraform provider requirements.
* 🔍 Query existing cloud resources using data sources.
* 🚀 Execute the Terraform lifecycle workflow.

---

# Prerequisites

Before starting, ensure:

* ✅ Terraform version 1.5.0 or higher installed.
* ✅ AWS CLI configured.
* ✅ Azure CLI installed and authenticated.
* ✅ Cloud permissions available.
* ✅ Previous Terraform setup completed.

---

# Terraform Multi-Cloud Architecture

Terraform can manage multiple cloud platforms from a single configuration.

```text id="4m8q7v"
              Terraform Configuration
                       |
        ┌──────────────┴──────────────┐
        ↓                             ↓
      AWS Provider              Azure Provider
        ↓                             ↓
 AWS Resources              Azure Resources
```

---

# Procedure

# Part 1 — Create Terraform Configuration

## Step 1 — Create Terraform File

Create:

```text id="8q3n5m"
main.tf
```

Add the following configuration:

```hcl id="6p9m2x"
terraform {

  required_version = ">= 1.5.0"

  required_providers {

    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }

    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0"
    }

  }

}


provider "aws" {

  region = var.aws_region

}


provider "azurerm" {

  features {}

}


variable "aws_region" {

  description = "AWS region"

  type = string

  default = "ap-south-1"

}


# Data source — query existing resources

data "aws_availability_zones" "available" {

  state = "available"

}


output "aws_azs" {

  value = data.aws_availability_zones.available.names

}
```

---

# Configuration Explanation

## Terraform Version Requirement

```hcl id="2v7m9q"
required_version = ">= 1.5.0"
```

Ensures compatibility with:

* Terraform CLI version.
* Required language features.
* Provider compatibility.

---

# Provider Configuration

## AWS Provider

```hcl id="9m4p6q"
provider "aws" {
  region = var.aws_region
}
```

Connects Terraform to:

```text id="5x8n2v"
Amazon Web Services
```

using the configured AWS region.

Default:

```text id="7q3m8p"
ap-south-1
```

---

## Azure Provider

```hcl id="1m8p5x"
provider "azurerm" {

  features {}

}
```

Enables Terraform management of:

```text id="8v5m2x"
Microsoft Azure resources
```

---

# Variable Configuration

## AWS Region Variable

```hcl id="4n7m9x"
variable "aws_region" {

  description = "AWS region"

  type = string

  default = "ap-south-1"

}
```

Benefits:

* Avoids hardcoding values.
* Supports multiple environments.
* Allows easy customization.

---

# Data Sources

## Query AWS Availability Zones

Configuration:

```hcl id="6m3q8p"
data "aws_availability_zones" "available" {

  state = "available"

}
```

Purpose:

* Retrieves existing AWS availability zones.
* Does not create resources.
* Provides information to Terraform.

---

# Output Values

Configuration:

```hcl id="9p5m2v"
output "aws_azs" {

  value = data.aws_availability_zones.available.names

}
```

Displays:

* Available AWS Availability Zones.
* Retrieved cloud information.

---

# Part 2 — Execute Terraform Workflow

Terraform follows a standard lifecycle:

```text id="3q8m6v"
Write Configuration
        |
        ↓
terraform init
        |
        ↓
terraform validate
        |
        ↓
terraform plan
        |
        ↓
terraform apply
        |
        ↓
terraform destroy
```

---

# Terraform Commands

## Step 2 — Initialize Terraform

Run:

```bash id="5m2q8v"
terraform init
```

Purpose:

* Downloads providers.
* Initializes working directory.
* Prepares backend configuration.

---

## Step 3 — Validate Configuration

Run:

```bash id="7v2m9q"
terraform validate
```

Purpose:

* Checks Terraform syntax.
* Detects configuration errors.

---

## Step 4 — Review Execution Plan

Run:

```bash id="2q8m5v"
terraform plan
```

Purpose:

* Shows planned infrastructure changes.
* Does not modify resources.

---

## Step 5 — Apply Configuration

Run:

```bash id="6x4m8p"
terraform apply -auto-approve
```

Purpose:

* Creates or updates infrastructure.
* Automatically approves execution.

---

## Step 6 — View Outputs

Run:

```bash id="9m3q7p"
terraform output
```

Displays:

* Terraform output values.
* Retrieved cloud information.

---

## Step 7 — Destroy Resources

Run:

```bash id="8p5m2v"
terraform destroy -auto-approve
```

Purpose:

* Removes Terraform-managed resources.

---

# Terraform Workflow Reference

| Command              | Purpose            |
| -------------------- | ------------------ |
| `terraform init`     | Initialize project |
| `terraform validate` | Check syntax       |
| `terraform plan`     | Preview changes    |
| `terraform apply`    | Deploy changes     |
| `terraform output`   | Display outputs    |
| `terraform destroy`  | Remove resources   |

---

# Best Practice Tips

> [!TIP]
> Follow these recommendations when using Terraform in cloud environments.

---

## ✅ Always Run terraform validate

Before:

```bash id="4q8m6v"
terraform plan
```

run:

```bash id="1v7m9q"
terraform validate
```

Benefits:

* Detects syntax problems early.
* Prevents failed deployments.
* Improves workflow reliability.

---

## 📄 Use terraform.tfvars for Environment Values

Avoid:

```hcl id="3m6q8p"
Hardcoded values inside main.tf
```

Use:

```text id="8p5m2v"
terraform.tfvars
```

Example:

```hcl id="5q9m3v"
aws_region = "ap-south-1"
```

Benefits:

* Supports multiple environments.
* Keeps configuration clean.
* Simplifies deployments.

---

## 🗄️ Use Remote State for Team Environments

Recommended:

```text id="2x8m5q"
S3 + DynamoDB
```

Benefits:

* Shared Terraform state.
* Prevents state conflicts.
* Enables team collaboration.

Remote state is mandatory for production team usage.

---

## ⚠️ Use -target Carefully

Example:

```bash id="6m8q2v"
terraform apply -target=aws_instance.web
```

Use only for:

* Recovery scenarios.
* Debugging.
* Specific resource operations.

Avoid regular use because it can create incomplete infrastructure states.

---

# Validation Checklist

Verify:

* ✅ Terraform version requirement configured.
* ✅ AWS provider configured.
* ✅ Azure provider configured.
* ✅ Variables created.
* ✅ Data source queried.
* ✅ Terraform initialized.
* ✅ Configuration validated.
* ✅ Plan reviewed.
* ✅ Apply and destroy workflows tested.

---

# Summary

In this lab, you:

* Configured Terraform for AWS and Azure.
* Defined multiple cloud providers.
* Used variables and data sources.
* Executed the Terraform workflow.
* Learned Terraform state and workflow best practices.

You are now ready to provision real cloud infrastructure using Terraform across multiple cloud platforms.
