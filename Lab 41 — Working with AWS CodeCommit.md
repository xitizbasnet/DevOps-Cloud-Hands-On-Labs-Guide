# Section 9 — AWS DevOps (Code Suite)

# Lab 41 — Working with AWS CodeCommit

> [!IMPORTANT]
> This lab demonstrates how to work with AWS CodeCommit as a Git-based source control repository. You will create a repository, configure Git authentication, push application code, and apply branch protection rules.

> [!NOTE]
> AWS announced that CodeCommit is no longer available for new customers as of July 2024. Existing customers may continue using it. For new projects, consider alternatives such as GitHub or AWS CodeCatalyst.

---

# Objective

Configure AWS CodeCommit to:

* 📁 Create a source code repository.
* 🔐 Configure HTTPS Git credentials.
* 📤 Clone and push application code.
* 🌿 Apply branch approval rules.
* 🔒 Follow source control security practices.

---

# Prerequisites

Before starting, ensure:

* ✅ AWS account available.
* ✅ IAM user or role configured.
* ✅ Git installed.
* ✅ AWS Region configured as `ap-south-1`.
* ✅ Basic Git workflow knowledge.

---

# AWS CodeCommit Workflow Overview

```text id="8m4q2v"
Developer
    |
    ↓
Git Repository
    |
    ↓
AWS CodeCommit
    |
    ↓
Pull Request Review
    |
    ↓
Approved Merge
```

---

# Part 1 — Create CodeCommit Repository

## Step 1 — Create Repository

Navigate:

```text id="4q8m2v"
AWS Console
    ↓
CodeCommit
    ↓
Create Repository
```

Create repository:

```text id="6m3q9v"
Repository Name:

DevOps-App
```

Purpose:

* Stores application source code.
* Provides Git-based version control.
* Integrates with AWS DevOps services.

---

# Part 2 — Configure Git Authentication

## Step 2 — Generate HTTPS Git Credentials

Navigate:

```text id="7v2m5q"
IAM
    ↓
Users
    ↓
Your User
    ↓
Security Credentials
    ↓
HTTPS Git Credentials for CodeCommit
    ↓
Generate Credentials
```

Purpose:

* Allows Git clients to authenticate with CodeCommit.
* Provides secure HTTPS repository access.

---

# Part 3 — Clone and Push Repository

## Step 3 — Clone Repository

Run:

```bash id="2m8q5v"
git clone https://git-codecommit.ap-south-1.amazonaws.com/v1/repos/DevOps-App
```

Navigate:

```bash id="5q9m3v"
cd DevOps-App
```

---

## Step 4 — Create Application README

Create file:

```bash id="8v3m6q"
echo '# My DevOps App' > README.md
```

---

## Step 5 — Commit Changes

Run:

```bash id="9m4q2v"
git add .
```

Commit:

```bash id="3q7m8v"
git commit -m 'Initial commit'
```

---

## Step 6 — Push Code

Run:

```bash id="6m2q9v"
git push origin main
```

The code is now available in:

```text id="4v8m3q"
AWS CodeCommit Repository
        ↓
DevOps-App
```

---

# Part 4 — Configure Branch Protection

## Step 7 — Add Approval Rules

Navigate:

```text id="5m8q2v"
CodeCommit
    ↓
Settings
    ↓
Approval rule templates
```

Configure:

```text id="7q3m9v"
Branch:

main


Requirement:

1 Approver
```

Purpose:

* Prevents unauthorized changes.
* Requires code review before merging.
* Improves development governance.

---

# Branch Protection Workflow

```text id="8q4m2v"
Developer Creates Branch

        ↓

Code Changes

        ↓

Pull Request

        ↓

Reviewer Approval

        ↓

Merge to main
```

---

# Best Practice Tips

> [!TIP]
> Follow these recommendations when managing CodeCommit repositories.

---

## 🔐 Use IAM Roles Instead of User Credentials

Recommended:

```text id="2m7q5v"
EC2 Instance
      |
      ↓
IAM Role
      |
      ↓
CodeCommit Access
```

Avoid:

```text id="6v9m3q"
Hardcoded IAM User Credentials
```

Benefits:

* Better security.
* Automatic credential rotation.
* Reduced credential exposure.

---

## 🔔 Enable Repository Notifications

Configure:

```text id="3q8m5v"
CodeCommit
    ↓
Notifications
    ↓
SNS
```

Use for:

* Pull request events.
* Repository activity.
* Approval notifications.

---

## 🌿 Use Git-Flow Branching Strategy

Recommended structure:

```text id="7m2q8v"
main
 |
 ├── develop
 |
 ├── feature/*
 |
 ├── release/*
 |
 └── hotfix/*
```

Benefits:

* Controlled releases.
* Clear development workflow.
* Better collaboration.

---

## ⚠️ AWS CodeCommit Availability Note

Important:

```text id="9q5m3v"
AWS CodeCommit is deprecated for new customers
(July 2024)
```

For new projects, consider:

* GitHub.
* AWS CodeCatalyst.
* Other Git hosting platforms.

---

# Validation Checklist

Verify:

* ✅ CodeCommit repository created.
* ✅ HTTPS Git credentials generated.
* ✅ Repository cloned successfully.
* ✅ README file committed.
* ✅ Code pushed successfully.
* ✅ Branch approval rule configured.
* ✅ Repository notifications reviewed.

---

# Summary

In this lab, you:

* Created an AWS CodeCommit repository.
* Configured Git authentication.
* Uploaded source code using Git commands.
* Added branch approval controls.
* Learned AWS source control security practices.

This provides the foundation for integrating AWS source repositories with CI/CD services such as AWS CodePipeline and CodeBuild.
