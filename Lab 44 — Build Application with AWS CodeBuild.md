# Lab 44 — Build Application with AWS CodeBuild

> [!IMPORTANT]
> This lab demonstrates how to create an AWS CodeBuild project for a Node.js application using a `buildspec.yml` configuration file.

> [!NOTE]
> AWS CodeBuild provides managed build environments that automatically compile source code, run tests, build container images, and publish artifacts.

---

# Objective

Configure AWS CodeBuild to:

* 🔨 Create an automated build process.
* 📦 Install Node.js application dependencies.
* 🧪 Execute application tests.
* 🐳 Build and publish Docker images to Amazon ECR.
* 🔐 Retrieve sensitive values securely using AWS Secrets Manager.
* 📤 Generate deployment artifacts.

---

# Prerequisites

Before starting, ensure:

* ✅ AWS CodeCommit repository configured.
* ✅ Node.js application source code available.
* ✅ Amazon ECR repository created.
* ✅ IAM permissions available for:

  * CodeBuild
  * Amazon ECR
  * Amazon S3
  * AWS Secrets Manager
* ✅ Docker configuration available for the application.

---

# AWS CodeBuild Workflow Overview

```text
Developer Push
      |
      ↓
AWS CodeCommit
      |
      ↓
AWS CodeBuild
      |
      ├── Install Dependencies
      |
      ├── Run Tests
      |
      ├── Build Docker Image
      |
      ├── Push Image to ECR
      |
      ↓
Build Artifacts Generated
```

---

# Step 1 — Create buildspec.yml

Create the following file in the repository root:

```yaml
# buildspec.yml

version: 0.2

env:
  variables:
    NODE_ENV: "production"

  secrets-manager:
    DB_PASSWORD: "prod/app/db:password"

phases:

  install:
    runtime-versions:
      nodejs: 18

    commands:
      - echo Installing dependencies
      - npm ci

  pre_build:
    commands:
      - echo Running tests
      - npm test

      - echo Logging in to Amazon ECR...
      - aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin $ECR_REPO

  build:
    commands:
      - echo Building Docker image
      - docker build -t myapp:$CODEBUILD_BUILD_NUMBER .
      - docker tag myapp:$CODEBUILD_BUILD_NUMBER $ECR_REPO/myapp:latest

  post_build:
    commands:
      - docker push $ECR_REPO/myapp:latest
      - echo Build completed on `date`

artifacts:
  files:
    - appspec.yml
    - scripts/**/*
  discard-paths: no

cache:
  paths:
    - node_modules/**/*
```

---

# buildspec.yml Section Reference

| Section      | Purpose                                    |
| ------------ | ------------------------------------------ |
| `version`    | Defines CodeBuild buildspec version        |
| `env`        | Stores environment variables and secrets   |
| `install`    | Installs required runtime and dependencies |
| `pre_build`  | Runs validation and authentication tasks   |
| `build`      | Creates application artifacts or images    |
| `post_build` | Publishes outputs after successful build   |
| `artifacts`  | Defines files passed to deployment         |
| `cache`      | Improves build performance                 |

---

# Step 2 — Create AWS CodeBuild Project

Navigate:

```text
AWS Console
    ↓
CodeBuild
    ↓
Create Build Project
```

Configure:

```text
Source Provider:

AWS CodeCommit
```

Select:

```text
Repository:

Your Application Repository
```

---

# Step 3 — Configure Build Environment

Select:

```text
Environment:

Managed Image
```

Operating system:

```text
Ubuntu
```

Purpose:

* Provides AWS-managed build servers.
* Supports Node.js and Docker workloads.
* Removes infrastructure management overhead.

---

# Step 4 — Configure Service Role Permissions

The CodeBuild service role must allow access to:

```text
Amazon ECR
Amazon S3
AWS Secrets Manager
CloudWatch Logs
```

Example permissions:

```text
CodeBuild
    |
    ├── Pull Source
    |
    ├── Read Secrets
    |
    ├── Build Application
    |
    └── Push Docker Image
```

---

# Step 5 — Start Build Execution

Start the build:

```text
CodeBuild
    ↓
Start Build
```

Monitor:

```text
CodeBuild
    ↓
Build History
    ↓
Build Logs
```

Expected output:

```text
Installing dependencies
Running tests
Building Docker image
Pushing image to ECR
Build completed successfully
```

---

# Build Artifact Flow

```text
Source Code
     |
     ↓
CodeBuild
     |
     ├── Test Results
     |
     ├── appspec.yml
     |
     └── Deployment Scripts
```

---

# Best Practice Tips

> [!TIP]
> Apply these recommendations for production CodeBuild projects.

---

## ⚡ Enable Build Cache

Use:

```yaml
cache:
  paths:
    - node_modules/**/*
    - .m2/repository/**/*
```

Benefits:

* Reduces dependency download time.
* Improves build speed.
* Lowers build costs.

---

## 🧪 Always Fail Builds on Test Errors

Recommended:

```text
Build
 ↓
Install
 ↓
Test
 ↓
Package
```

Do not skip failed tests.

Benefits:

* Prevents defective releases.
* Improves application reliability.

---

## 🔐 Store Secrets Securely

Use:

```text
AWS Secrets Manager
        ↓
CodeBuild Environment Variables
```

Avoid:

```text
Hardcoded passwords
API keys
Database credentials
```

---

## 📊 Publish Build Reports

Enable:

```text
CodeBuild
    ↓
Reports
    ↓
Test Results
```

Benefits:

* Better visibility.
* Historical test tracking.
* Easier troubleshooting.

---

# Validation Checklist

Verify:

* ✅ `buildspec.yml` created in repository root.
* ✅ Node.js runtime configured.
* ✅ Dependencies installed successfully.
* ✅ Tests executed successfully.
* ✅ Docker image built.
* ✅ Docker image pushed to ECR.
* ✅ Build artifacts generated.
* ✅ Build logs available in AWS console.

---

# Summary

In this lab, you:

* Created an AWS CodeBuild configuration.
* Automated Node.js application builds.
* Integrated Docker image creation.
* Published images to Amazon ECR.
* Applied secure secret management practices.

This CodeBuild project can now be connected with AWS CodePipeline for a complete CI/CD workflow.
