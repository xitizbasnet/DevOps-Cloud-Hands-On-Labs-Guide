# Section 7 — Terraform

# Lab 31 — Installing Terraform Client on Windows

> [!IMPORTANT]
> This lab demonstrates how to install **Terraform on Windows**, configure AWS authentication, initialize an AWS Terraform provider, and verify that the Terraform environment is ready for infrastructure automation.

## Objective

Install Terraform on Windows and configure the AWS provider.

By the end of this lab, you will:

* 🛠️ Install Terraform CLI.
* ⚙️ Configure Windows environment variables.
* ☁️ Install and configure AWS CLI.
* 🔑 Configure AWS credentials.
* 🚀 Initialize a Terraform AWS provider project.

---

# Prerequisites

Before starting, ensure:

* ✅ Windows operating system.
* ✅ Administrator access.
* ✅ AWS account available.
* ✅ AWS IAM credentials created.
* ✅ Internet connectivity.

---

# Terraform Workflow Overview

Infrastructure automation flow:

```text id="4m8q7v"
Terraform CLI Installation
          |
          ↓
Configure AWS Credentials
          |
          ↓
Create Terraform Configuration
          |
          ↓
terraform init
          |
          ↓
Deploy Infrastructure
```

---

# Part 1 — Install Terraform on Windows

## Step 1 — Download Terraform

Download Terraform from:

```text id="8q3n5m"
https://developer.hashicorp.com/terraform/downloads
```

Select:

```text id="6p9m2x"
Windows AMD64 ZIP
```

---

## Step 2 — Extract Terraform Binary

Extract:

```text id="2v7m9q"
terraform.exe
```

to:

```text id="9m4p6q"
C:\terraform\
```

Example:

```text id="5x8n2v"
C:\terraform\terraform.exe
```

---

## Step 3 — Add Terraform to System PATH

Navigate:

```text id="7q3m8p"
System Properties
        ↓
Environment Variables
        ↓
Path
        ↓
Add C:\terraform\
```

This allows Terraform to run from any PowerShell location.

---

## Step 4 — Verify Terraform Installation

Open a new PowerShell window:

```powershell id="1m8p5x"
terraform version
```

Expected output:

```text id="8v5m2x"
Terraform vX.X.X
```

---

# Part 2 — Install AWS CLI

## Step 5 — Download AWS CLI

Download:

```text id="4n7m9x"
https://aws.amazon.com/cli/
```

Install:

```text id="6m3q8p"
AWS CLI MSI Installer
```

---

## Step 6 — Verify AWS CLI Installation

Run:

```powershell id="9p5m2v"
aws --version
```

Example:

```text id="3q8m6v"
aws-cli/2.x.x
```

---

# Part 3 — Configure AWS Credentials

## Step 7 — Configure AWS CLI

Run:

```powershell id="5m2q8v"
aws configure
```

Enter:

```text id="7v2m9q"
AWS Access Key ID: AKIA...
AWS Secret Access Key: ...
Default region name: ap-south-1
Default output format: json
```

Configuration details:

| Setting           | Value              |
| ----------------- | ------------------ |
| Access Key ID     | AWS IAM access key |
| Secret Access Key | AWS IAM secret key |
| Region            | `ap-south-1`       |
| Output Format     | `json`             |

---

# Part 4 — Test Terraform AWS Provider

## Step 8 — Create Terraform Lab Directory

Open PowerShell:

```powershell id="2q8m5v"
mkdir C:\tf-labs\lab31
cd C:\tf-labs\lab31
```

---

## Step 9 — Create Terraform Configuration

Create:

```text id="6x4m8p"
main.tf
```

Add:

```hcl id="9m3q7p"
provider "aws" {
  region = "ap-south-1"
}
```

---

## Step 10 — Initialize Terraform

Run:

```powershell id="8p5m2v"
terraform init
```

Terraform will:

* Download required provider plugins.
* Initialize the working directory.
* Prepare the project for deployment.

---

## Step 11 — Verify Terraform Providers

Run:

```powershell id="4q8m6v"
terraform providers
```

Expected:

```text id="1v7m9q"
provider[registry.terraform.io/hashicorp/aws]
```

---

# Terraform Project Structure

After setup:

```text id="3m6q8p"
C:\tf-labs\lab31
│
└── main.tf
```

---

# Useful Terraform Commands

| Command               | Purpose                      |
| --------------------- | ---------------------------- |
| `terraform version`   | Check Terraform installation |
| `terraform init`      | Initialize project           |
| `terraform providers` | Display providers            |
| `terraform validate`  | Validate configuration       |
| `terraform plan`      | Preview changes              |
| `terraform apply`     | Deploy infrastructure        |
| `terraform destroy`   | Remove infrastructure        |

---

# Best Practice Tips

> [!TIP]
> Follow these recommendations when preparing Terraform environments.

---

## 🔄 Use Terraform Version Managers

Instead of manually installing versions, use:

```text id="5p8m2v"
tfenv
```

Benefits:

* Supports multiple Terraform versions.
* Simplifies version switching.
* Helps maintain project compatibility.

---

## 🔐 Never Store AWS Credentials in Terraform Files

Do not store:

```text id="7m4q9p"
AWS Access Keys
AWS Secret Keys
```

inside:

```text id="8x2m5v"
.tf
```

files.

Use:

* `aws configure`.
* IAM roles.
* Environment variables.
* AWS profiles.

---

## 💻 Install Terraform Extension in VS Code

Install:

```text id="2x8m5q"
HashiCorp Terraform Extension
```

Benefits:

* Syntax highlighting.
* Auto-completion.
* Configuration validation.
* Better Terraform development experience.

---

## 🐧 Use Terraform Natively on WSL2

For Windows users using WSL2:

Install Terraform inside Linux environment.

Benefits:

* Better command-line experience.
* Improved compatibility.
* Native Linux tooling support.

---

# Troubleshooting Guide

## Terraform Command Not Found

Problem:

```text
terraform is not recognized
```

Solution:

* Verify `C:\terraform\` exists.
* Check PATH configuration.
* Open a new PowerShell window.

---

## AWS Authentication Failure

Check AWS configuration:

```powershell id="6q9m8p"
aws configure list
```

Verify:

* Access key.
* Secret key.
* Region.

---

# Validation Checklist

Verify:

* ✅ Terraform downloaded.
* ✅ Terraform added to PATH.
* ✅ Terraform version displayed.
* ✅ AWS CLI installed.
* ✅ AWS credentials configured.
* ✅ AWS provider initialized.
* ✅ Terraform providers verified.

---

# Summary

In this lab, you:

* Installed Terraform CLI on Windows.
* Configured Terraform environment variables.
* Installed AWS CLI.
* Configured AWS authentication.
* Created an AWS Terraform provider configuration.
* Initialized a Terraform project.

Your Windows Terraform environment is now ready for infrastructure-as-code labs and AWS resource provisioning.
