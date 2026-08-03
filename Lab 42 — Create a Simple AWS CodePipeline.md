# Lab 42 — Create a Simple AWS CodePipeline

> [!IMPORTANT]
> This lab demonstrates how to create an automated AWS CI/CD pipeline by connecting source control, build automation, and deployment services.

> [!NOTE]
> The pipeline workflow in this lab connects:
>
> * 📁 AWS CodeCommit — Source repository
> * 🔨 AWS CodeBuild — Build automation
> * 🚀 AWS CodeDeploy — Application deployment

---

# Objective

Create an automated CI/CD pipeline that:

* 🔗 Connects AWS CodeCommit, CodeBuild, and CodeDeploy.
* ⚙️ Automatically builds and deploys application changes.
* 🔄 Triggers pipeline execution when code is pushed to the main branch.
* 📦 Provides a foundation for production deployment workflows.

---

# Prerequisites

Before starting, ensure:

* ✅ AWS account configured.
* ✅ AWS CodeCommit repository created (Lab 41).
* ✅ Application source code available in the repository.
* ✅ Required IAM permissions available.
* ✅ AWS Region configured.

---

# AWS CodePipeline Workflow Overview

```text
Developer
    |
    ↓
Git Push
    |
    ↓
AWS CodeCommit
    |
    ↓
AWS CodePipeline Trigger
    |
    ↓
AWS CodeBuild
    |
    ↓
AWS CodeDeploy
    |
    ↓
Application Deployment
```

---

# Steps

## Step 1 — Create CodePipeline

Navigate:

```text
AWS Console
    ↓
CodePipeline
    ↓
Create Pipeline
```

Configure:

```text
Pipeline Name:

DevOps-Pipeline
```

---

## Step 2 — Configure Service Role

Select:

```text
Service Role:

Create new role
```

AWS automatically creates the required IAM service role.

Purpose:

* Allows CodePipeline to access AWS services.
* Provides permissions for source, build, and deployment stages.

---

# Step 3 — Configure Source Stage

Select:

```text
Source Provider:

AWS CodeCommit
```

Configure repository:

```text
Repository:

DevOps-App


Branch:

main
```

Enable change detection:

```text
Detection Method:

CloudWatch Events
```

Purpose:

* Automatically detects new commits.
* Starts pipeline execution after Git push.

---

# Step 4 — Configure Build Stage

Select:

```text
Build Provider:

AWS CodeBuild
```

Create or select a CodeBuild project.

> [!NOTE]
> CodeBuild project creation is covered in **Lab 44**.

Purpose:

* Compile application code.
* Run automated tests.
* Generate build artifacts.

---

# Step 5 — Configure Deploy Stage

Select:

```text
Deploy Provider:

AWS CodeDeploy
```

Configure deployment settings.

> [!NOTE]
> CodeDeploy configuration is covered in **Lab 45**.

Purpose:

* Deploy application artifacts.
* Automate application release process.

---

# Step 6 — Review and Create Pipeline

Review all configured stages:

```text
Source
   ↓
Build
   ↓
Deploy
```

Select:

```text
Create Pipeline
```

AWS will create and start the pipeline execution.

---

# Step 7 — Test Automatic Trigger

Push a code change:

```bash
git add .
git commit -m "Update application"
git push origin main
```

Expected behavior:

```text
Git Push
    ↓
CodeCommit detects change
    ↓
CodePipeline starts automatically
    ↓
Build executes
    ↓
Deployment runs
```

---

# Pipeline Stage Reference

| Stage  | AWS Service    | Purpose                        |
| ------ | -------------- | ------------------------------ |
| Source | AWS CodeCommit | Stores application source code |
| Build  | AWS CodeBuild  | Compiles and tests application |
| Deploy | AWS CodeDeploy | Releases application           |

---

# Best Practice Tips

> [!TIP]
> Apply these recommendations for production-ready pipelines.

---

## 🔐 Add Manual Approval Before Production

Recommended workflow:

```text
Development
      ↓
Testing
      ↓
Manual Approval
      ↓
Production
```

Benefits:

* Prevents accidental production releases.
* Enables change review.
* Supports compliance requirements.

---

## ⚙️ Use Pipeline Variables

Use environment-specific variables:

```text
Development:

DATABASE_URL=dev-db


Production:

DATABASE_URL=prod-db
```

Benefits:

* Same pipeline for multiple environments.
* Cleaner configuration management.
* Easier deployments.

---

## 🔔 Enable Pipeline Notifications

Configure:

```text
CodePipeline
      ↓
Notifications
      ↓
SNS / Slack Integration
```

Use notifications for:

* Pipeline failures.
* Successful deployments.
* Approval requests.

---

## 🔗 Connect GitHub Using CodeStar Connections

For GitHub-based workflows:

```text
GitHub
   ↓
CodeStar Connections
   ↓
CodePipeline
```

Benefits:

* Native GitHub integration.
* Secure authentication.
* Automated CI/CD triggers.

---

# Validation Checklist

Verify:

* ✅ CodePipeline created successfully.
* ✅ CodeCommit connected as source.
* ✅ CodeBuild configured.
* ✅ CodeDeploy configured.
* ✅ Pipeline execution completed successfully.
* ✅ Git push automatically triggers pipeline.

---

# Summary

In this lab, you:

* Created an AWS CodePipeline workflow.
* Connected source, build, and deployment stages.
* Configured automatic pipeline triggers.
* Learned CI/CD automation best practices.

This pipeline provides the foundation for enterprise AWS DevOps delivery workflows.
