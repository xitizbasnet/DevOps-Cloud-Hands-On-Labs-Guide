# Section 8 — Azure DevOps

# Lab 36 — Create Azure DevOps FREE Account

> [!IMPORTANT]
> This lab introduces Azure DevOps setup, including organization creation, project configuration, Git repository initialization, Agile work management, and exploration of Azure DevOps service modules.

## Objective

Set up:

* 🏢 Azure DevOps organization.
* 📁 Azure DevOps project.
* 📦 Git repository.
* 📋 Agile work item hierarchy.
* 🚀 Azure DevOps service modules.

By the end of this lab, you will understand the basic Azure DevOps platform structure and workflow.

---

# Prerequisites

Before starting, ensure:

* ✅ Microsoft account available.
* ✅ Internet connectivity.
* ✅ Basic Git knowledge.
* ✅ Understanding of Agile project management concepts.

---

# Azure DevOps Overview

Azure DevOps provides integrated services for:

```text id="7m3q8v"
Azure DevOps Platform

        |
 ┌──────┼──────┐
 ↓      ↓      ↓

 Boards  Repos  Pipelines

        |
 ┌──────┴──────┐

 Test Plans  Artifacts
```

---

# Part 1 — Create Azure DevOps Organization

## Step 1 — Open Azure DevOps

Navigate to:

```text id="8q3m5v"
https://dev.azure.com
```

Sign in using:

```text id="4p7m9x"
Microsoft Account
```

---

## Step 2 — Create Organization

Create organization:

```text id="6m2q8v"
DevOpsVinod
```

Select region:

```text id="9v4m3q"
India
```

Organization represents the top-level container for:

* Projects.
* Users.
* Security settings.
* Billing.

---

# Part 2 — Create Azure DevOps Project

## Step 3 — Create Project

Create:

```text id="2m8q5v"
Project Name:
DevOps-Labs
```

Configure:

| Setting           | Value   |
| ----------------- | ------- |
| Visibility        | Private |
| Version Control   | Git     |
| Work Item Process | Agile   |

---

# Project Structure

After creation:

```text id="5q8m3v"
Azure DevOps Organization

        |
        ↓

     DevOps-Labs

        |
 ┌──────┼──────┬──────┬──────┐
 ↓      ↓      ↓      ↓

Boards Repos Pipelines Test Plans Artifacts
```

---

# Part 3 — Explore Azure DevOps Services

## Step 4 — Explore Left Navigation

Open:

```text id="8m4q2v"
Azure DevOps Left Navigation
```

Explore:

| Service    | Purpose                     |
| ---------- | --------------------------- |
| Boards     | Agile planning and tracking |
| Repos      | Git source control          |
| Pipelines  | CI/CD automation            |
| Test Plans | Testing management          |
| Artifacts  | Package management          |

---

# Part 4 — Configure Repository

## Step 5 — Initialize Repository

Navigate:

```text id="3q7m9v"
Repos → Initialize with README
```

This creates:

```text id="6v2m8q"
README.md
```

inside the Git repository.

---

# Part 5 — Configure Agile Work Items

## Step 6 — Create Work Item Hierarchy

Navigate:

```text id="1m8q5v"
Boards → Work Items
```

Create hierarchy:

```text id="7p3m9x"
Epic
 |
 └── Feature
       |
       └── User Story
              |
              └── Task
```

Purpose:

* Organize requirements.
* Track development progress.
* Support Agile planning.

---

# Part 6 — Configure Billing

## Step 7 — Start Free Plan

Navigate:

```text id="5m9q2v"
Organization Settings
        ↓
Billing
        ↓
Start Free
```

Free tier includes:

```text id="8x4m7p"
5 users free

1800 CI/CD minutes free per month
```

---

# Azure DevOps Services Reference

| Service          | Usage                                 |
| ---------------- | ------------------------------------- |
| Azure Boards     | Sprint planning and work tracking     |
| Azure Repos      | Git repository hosting                |
| Azure Pipelines  | Continuous Integration and Deployment |
| Azure Test Plans | Manual and automated testing          |
| Azure Artifacts  | Package feeds                         |

---

# Best Practice Tips

> [!TIP]
> Follow these recommendations when implementing Azure DevOps workflows.

---

## 🔗 Use Azure Repos + Azure Pipelines Together

Recommended workflow:

```text id="2q7m5v"
Developer Commit
        |
        ↓
Azure Repos
        |
        ↓
Azure Pipelines
        |
        ↓
Build & Deployment
```

Benefits:

* Native integration.
* Automated workflows.
* Better security controls.

---

## 🌿 Configure Main Branch Policies

Protect:

```text id="6m3q8v"
main branch
```

Recommended rules:

* Require Pull Request.
* Require minimum 1 reviewer.
* Enable build validation.

Benefits:

* Prevents direct unsafe changes.
* Improves code quality.

---

## 📋 Use Azure Boards for Agile Planning

Recommended for teams using:

* Agile.
* Scrum.
* Sprint-based development.

Benefits:

* Requirement tracking.
* Sprint planning.
* Progress visibility.

---

## 🆓 Azure DevOps Free Tier

Includes:

```text id="9q4m7v"
5 users free

Unlimited private Git repositories
```

Benefits:

* Suitable for learning.
* Supports small development teams.
* Enables CI/CD practice.

---

# Validation Checklist

Verify:

* ✅ Azure DevOps organization created.
* ✅ Region selected.
* ✅ Project created successfully.
* ✅ Git repository initialized.
* ✅ Boards hierarchy created.
* ✅ Azure DevOps modules explored.
* ✅ Free tier enabled.

---

# Summary

In this lab, you:

* Created an Azure DevOps organization.
* Created a private project.
* Initialized a Git repository.
* Explored Boards, Repos, Pipelines, Test Plans, and Artifacts.
* Configured Agile work item management.

Your Azure DevOps environment is now ready for source control, CI/CD automation, and project collaboration.
