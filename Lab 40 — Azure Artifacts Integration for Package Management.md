# Lab 40 — Azure Artifacts Integration for Package Management

> [!IMPORTANT]
> This lab demonstrates how to use Azure Artifacts to host and manage private packages. You will create an Azure Artifacts feed, configure npm authentication, publish packages, and integrate package restoration into Azure Pipelines.

---

# Objective

Configure Azure Artifacts to:

* 📦 Create private package feeds.
* 🔐 Authenticate package access.
* 📤 Publish npm packages.
* 🔄 Restore packages during CI/CD pipelines.
* 🛡️ Control external package ingestion.

---

# Prerequisites

Before starting, ensure:

* ✅ Azure DevOps project available.
* ✅ Node.js and npm installed.
* ✅ Azure DevOps permissions available.
* ✅ CI/CD pipeline configured.
* ✅ npm package project available.

---

# Azure Artifacts Workflow Overview

```text id="7m4q8v"
Developer Package
        |
        ↓
Azure Artifacts Feed
        |
        ↓
Azure Pipeline Authentication
        |
        ↓
Package Restore
        |
        ↓
Application Build
```

---

# Part 1 — Create Azure Artifacts Feed

## Step 1 — Create Feed

Navigate:

```text id="3q8m5v"
Azure DevOps
        ↓
Artifacts
        ↓
Create Feed
```

Configure:

| Setting    | Value           |
| ---------- | --------------- |
| Feed Name  | devops-packages |
| Visibility | Private         |

Create:

```text id="8v2m6q"
devops-packages
```

Purpose:

* Store internal packages.
* Control package access.
* Manage package versions.

---

# Part 2 — Connect npm Client to Azure Artifacts

## Step 2 — Connect to Feed

Navigate:

```text id="5m9q3v"
Artifacts
        ↓
devops-packages
        ↓
Connect to Feed
        ↓
npm
```

Follow the provided setup instructions.

---

# Part 3 — Configure npm Authentication

## Install Azure Artifacts Authentication Tool

Run:

```bash id="4q7m9v"
npm install -g vsts-npm-auth
```

Authenticate:

```bash id="6m3q8v"
vsts-npm-auth -config .npmrc
```

Purpose:

* Authenticates npm client.
* Enables private package access.

---

# Configure `.npmrc`

Create or update:

```text id="9m5q2v"
.npmrc
```

Add:

```ini id="2v8m4q"
registry=https://pkgs.dev.azure.com/DevOpsVinod/DevOps-Labs/_packaging/devops-packages/npm/registry/
```

This configures npm to use Azure Artifacts as the package registry.

---

# Part 4 — Publish npm Package

## Publish Package

Run:

```bash id="7q3m8v"
npm publish
```

The package will be uploaded to:

```text id="5v9m2q"
Azure DevOps
        ↓
Artifacts
        ↓
devops-packages
```

---

# Part 5 — Integrate Azure Artifacts with Pipeline

## Step 1 — Add npm Authentication Task

Add the following step to your Azure Pipeline:

```yaml id="8m2q5v"
- task: npmAuthenticate@0

  inputs:

    workingFile: .npmrc


- script: npm ci

  displayName: 'Install from Artifacts feed'
```

---

# Pipeline Package Flow

```text id="3m8q2v"
Azure Pipeline Agent

        |
        ↓

npmAuthenticate Task

        |
        ↓

Azure Artifacts Feed

        |
        ↓

npm ci

        |
        ↓

Application Build
```

---

# Azure Artifacts Features

| Feature            | Purpose                     |
| ------------------ | --------------------------- |
| Private Feeds      | Store internal packages     |
| npm Support        | Manage JavaScript packages  |
| PyPI Support       | Manage Python packages      |
| Maven Support      | Manage Java packages        |
| Upstream Sources   | Proxy external repositories |
| Version Management | Control package releases    |

---

# Best Practice Tips

> [!TIP]
> Apply these recommendations for secure package management.

---

## 📦 Use Artifacts as a Package Proxy

Recommended usage:

```text id="6q9m3v"
External Registry
        |
        ↓
Azure Artifacts Feed
        |
        ↓
Application Pipeline
```

Supported ecosystems:

* npm.
* PyPI.
* Maven.

Benefits:

* Controlled package ingestion.
* Centralized dependency management.
* Improved security.

---

## 🧹 Configure Retention Policies

Recommended:

```text id="2m7q5v"
Automatically delete package versions older than 90 days
```

Benefits:

* Reduces storage usage.
* Keeps feeds clean.
* Improves package management.

---

## 🔢 Use Semantic Versioning

Recommended format:

```text id="5q8m9v"
MAJOR.MINOR.PATCH

Example:

1.2.3
```

Benefits:

* Clear release tracking.
* Dependency compatibility.
* Automated version validation.

---

## 🔒 Use Upstream Sources Securely

Configure upstream sources:

```text id="8m4q2v"
Docker Hub
npmjs.org
Other Public Registries
        |
        ↓
Azure Artifacts Feed
        |
        ↓
Internal Applications
```

Benefits:

* Security scanning.
* Controlled dependency flow.
* Reduced supply-chain risk.

---

# Validation Checklist

Verify:

* ✅ Azure Artifacts feed created.
* ✅ Feed visibility configured as private.
* ✅ npm authentication completed.
* ✅ `.npmrc` configured.
* ✅ Package published successfully.
* ✅ Pipeline restores packages from feed.
* ✅ Package versions managed correctly.

---

# Summary

In this lab, you:

* Created a private Azure Artifacts feed.
* Configured npm authentication.
* Published packages to Azure Artifacts.
* Integrated package restoration into Azure Pipelines.
* Learned secure package management practices.

Azure Artifacts provides a centralized and secure solution for managing application dependencies across enterprise CI/CD workflows.
