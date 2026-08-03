# Lab 46 — Create Simple CodePipeline + CodeStar

> [!IMPORTANT]
> This lab demonstrates an end-to-end AWS CI/CD pipeline workflow by integrating source control, build, testing, deployment, and project dashboard visibility.

> [!NOTE]
> AWS CodeStar was previously used to create complete DevOps project environments automatically. AWS has deprecated CodeStar; for new projects, use direct AWS service configuration or AWS CodeCatalyst.

---

# Objective

Build an automated CI/CD pipeline that includes:

* 📁 Source code management.
* 🔨 Automated application build.
* 🧪 Automated testing.
* 🚀 Application deployment.
* 📊 Project dashboard visibility.
* 🔔 Pipeline monitoring and notifications.

---

# Prerequisites

Before starting, ensure:

* ✅ AWS CodeCommit repository available.
* ✅ AWS CodeBuild project configured (Lab 44).
* ✅ AWS CodeDeploy deployment configured (Lab 45).
* ✅ IAM permissions configured.
* ✅ Application source code available.

---

# End-to-End CI/CD Workflow

```text id="1h5o7s"
Developer
    |
    ↓
Source Repository
(CodeCommit)
    |
    ↓
Build Stage
(CodeBuild)
    |
    ↓
Test Stage
(CodeBuild Tests)
    |
    ↓
Manual Approval
    |
    ↓
Deploy Stage
(CodeDeploy)
    |
    ↓
Production Environment
```

---

# Step 1 — Create Complete CodePipeline

Repeat the pipeline creation process from **Lab 42**.

Navigate:

```text id="3j7m9p"
AWS Console
    ↓
CodePipeline
    ↓
Create Pipeline
```

Configure all stages:

```text id="7x2b4m"
Source
    ↓
Build
    ↓
Test
    ↓
Deploy
```

Use:

| Stage  | AWS Service    | Lab Reference |
| ------ | -------------- | ------------- |
| Source | AWS CodeCommit | Lab 41        |
| Build  | AWS CodeBuild  | Lab 44        |
| Deploy | AWS CodeDeploy | Lab 45        |

---

# Step 2 — Add Manual Approval Stage

Insert an approval step:

```text id="8k4q2v"
Staging Deployment

        ↓

Manual Approval

        ↓

Production Deployment
```

Purpose:

* Provides release control.
* Allows testing before production.
* Supports enterprise change management.

Recommended approval users:

* Release managers.
* Application owners.
* Operations teams.

---

# Step 3 — Create AWS CodeStar Project

Navigate:

```text id="5p9r3d"
AWS CodeStar
    ↓
Create Project
```

Select template:

```text id="6m2q8v"
Node.js Web Application
```

Source provider:

```text id="9q3w5k"
AWS CodeCommit
```

---

# Step 4 — CodeStar Generated Resources

AWS CodeStar creates:

```text id="4v8m1q"
CodeStar Project
        |
        ├── CodeCommit Repository
        |
        ├── CodeBuild Project
        |
        ├── CodePipeline
        |
        └── CloudWatch Dashboard
```

Purpose:

* Provides centralized project visibility.
* Automates initial DevOps service setup.
* Simplifies pipeline creation.

---

# Step 5 — Test End-to-End Pipeline Execution

Modify application code:

```bash id="2q8m5v"
git add .
git commit -m "Update application"
git push origin main
```

Expected workflow:

```text id="7m4q9v"
Git Push

   ↓

Pipeline Trigger

   ↓

Build

   ↓

Test

   ↓

Approval

   ↓

Deployment

   ↓

Application Updated
```

---

# Step 6 — Trigger Pipeline Manually

Start pipeline execution:

```bash id="8q2m6v"
aws codepipeline start-pipeline-execution \
--name DevOps-Pipeline
```

---

# Step 7 — Check Pipeline Status

Retrieve pipeline state:

```bash id="3m9q7v"
aws codepipeline get-pipeline-state \
--name DevOps-Pipeline
```

Example output includes:

* Current stage status.
* Execution state.
* Failure details.
* Approval status.

---

# Pipeline Monitoring Architecture

```text id="9v3m5q"
AWS CodePipeline
        |
        ↓
CloudWatch Metrics
        |
        ↓
Dashboard
        |
        ↓
Team Visibility
```

---

# Best Practice Tips

> [!TIP]
> Apply these recommendations for production CI/CD pipelines.

---

## 🔔 Configure Pipeline Notifications

Add notifications using:

```text id="4m8q2v"
CodePipeline
    ↓
SNS Notifications
    ↓
Slack / Email
```

Notify teams about:

* Successful deployments.
* Pipeline failures.
* Approval requests.

---

## ⚙️ Use Pipeline-Level Variables

Example:

```text id="6q3m9v"
Environment:

Development
    FEATURE_FLAG=true


Production
    FEATURE_FLAG=false
```

Benefits:

* Environment-specific behavior.
* Easier release management.
* Cleaner pipeline configuration.

---

## ⚠️ AWS CodeStar Availability Note

Important:

```text id="8m5q1v"
AWS CodeStar is deprecated.
```

Recommended alternatives:

* AWS CodeCatalyst.
* Direct AWS service configuration:

  * CodeCommit.
  * CodeBuild.
  * CodeDeploy.
  * CodePipeline.

---

## 📈 Monitor DORA Metrics

Track:

| Metric                | Description                      |
| --------------------- | -------------------------------- |
| Deployment Frequency  | How often releases are deployed  |
| Lead Time for Changes | Time from commit to production   |
| Change Failure Rate   | Percentage of failed deployments |
| Mean Time to Recovery | Recovery speed after failure     |

Use:

```text id="5q7m2v"
CloudWatch
    ↓
Pipeline Metrics
    ↓
DORA Measurements
```

---

# Validation Checklist

Verify:

* ✅ Complete pipeline created.
* ✅ Source stage connected.
* ✅ Build stage executing successfully.
* ✅ Test stage configured.
* ✅ Manual approval enabled.
* ✅ Deployment stage completed.
* ✅ CodeStar resources reviewed.
* ✅ CloudWatch monitoring available.

---

# Summary

In this lab, you:

* Created an end-to-end AWS CI/CD pipeline.
* Connected source, build, test, and deployment stages.
* Added manual production approval.
* Explored CodeStar project automation.
* Learned pipeline monitoring and DevOps measurement practices.

This workflow represents a complete AWS DevOps delivery lifecycle.
