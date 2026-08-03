# Lab 37 — Build, Test, and Publish Artifacts Using Azure Pipelines

> [!IMPORTANT]
> This lab demonstrates how to create an Azure DevOps YAML pipeline that automatically builds application code, executes tests, and publishes build artifacts.

## Objective

Create an Azure DevOps CI/CD pipeline that:

* 🔄 Automatically triggers on code changes.
* 🏗️ Builds the application.
* 🧪 Executes automated tests.
* 📦 Publishes build artifacts.
* 🚀 Deploys the application to a development environment.

---

# Prerequisites

Before starting, ensure:

* ✅ Azure DevOps organization created.
* ✅ Azure DevOps project available.
* ✅ Source code repository created in Azure Repos.
* ✅ Application supports Node.js build commands.
* ✅ Basic YAML understanding.

---

# Pipeline Workflow Overview

The pipeline flow:

```text
Developer Commit
        |
        ↓
Azure Repos Git
        |
        ↓
Azure Pipeline Trigger
        |
        ↓
Build Stage
        |
        ├── Install Node.js
        ├── Install Dependencies
        ├── Build Application
        ├── Run Tests
        └── Publish Artifacts
        |
        ↓
Deploy Dev Environment
```

---

# Part 1 — Create Azure Pipeline

## Step 1 — Open Pipeline Creation

Navigate:

```text
Azure DevOps
    ↓
Pipelines
    ↓
New Pipeline
```

Select:

```text
Azure Repos Git
```

Choose your repository.

---

# Part 2 — Configure YAML Pipeline

## Step 2 — Select Starter Pipeline

Choose:

```text
Starter Pipeline
```

Replace the YAML content with:

```yaml
# azure-pipelines.yml

trigger:
  branches:
    include:
      - main
      - develop


pool:
  vmImage: 'ubuntu-latest'


variables:
  buildConfiguration: 'Release'
  appName: 'devops-app'


stages:

# ==========================
# Build Stage
# ==========================

- stage: Build

  displayName: 'Build Stage'

  jobs:

  - job: Build

    steps:

    - task: NodeTool@0

      inputs:

        versionSpec: '18.x'

      displayName: 'Install Node.js'


    - script: |

        npm ci

        npm run build

        npm test

      displayName: 'Install, Build & Test'


    - task: PublishTestResults@2

      inputs:

        testResultsFormat: 'JUnit'

        testResultsFiles: '**/test-results.xml'


    - task: PublishBuildArtifacts@1

      inputs:

        pathtoPublish: '$(Build.ArtifactStagingDirectory)'

        artifactName: 'drop'


# ==========================
# Deployment Stage
# ==========================

- stage: Deploy_Dev

  displayName: 'Deploy to Dev'


  dependsOn: Build


  condition: succeeded()


  jobs:


  - deployment: DeployDev

    environment: 'development'


    strategy:

      runOnce:

        deploy:

          steps:

          - script: |

              echo "Deploying to Dev environment"

            displayName: 'Deploy Step'
```

---

# Part 3 — Run Pipeline

## Step 3 — Save and Execute

Select:

```text
Save and Run
```

Azure DevOps will:

1. Commit the YAML pipeline file.
2. Start pipeline execution.
3. Allocate an Ubuntu build agent.
4. Execute build stages.

---

# Pipeline Stage Details

## Build Stage

### Install Node.js

Task:

```yaml
NodeTool@0
```

Purpose:

* Installs required Node.js version.
* Provides consistent build environment.

---

## Install Dependencies

Command:

```bash
npm ci
```

Purpose:

* Installs exact dependency versions.
* Provides repeatable builds.

---

## Build Application

Command:

```bash
npm run build
```

Purpose:

* Compiles application source code.
* Generates production build output.

---

## Execute Tests

Command:

```bash
npm test
```

Purpose:

* Validates application functionality.
* Detects issues before deployment.

---

## Publish Test Results

Task:

```yaml
PublishTestResults@2
```

Purpose:

* Uploads JUnit test reports.
* Provides test visibility in Azure DevOps.

---

## Publish Build Artifacts

Task:

```yaml
PublishBuildArtifacts@1
```

Artifact:

```text
drop
```

Purpose:

* Stores build output.
* Makes artifacts available for deployment stages.

---

# Deployment Stage

## Deploy Dev Environment

Deployment job:

```yaml
deployment: DeployDev
```

Environment:

```yaml
environment: 'development'
```

Execution:

```bash
echo "Deploying to Dev environment"
```

Purpose:

* Demonstrates deployment workflow.
* Provides a foundation for real deployment scripts.

---

# Monitoring Pipeline Execution

Monitor:

```text
Azure DevOps
    ↓
Pipelines
    ↓
Pipeline Run
```

Review:

* Build logs.
* Test results.
* Artifact publishing.
* Deployment status.

---

# Best Practice Tips

> [!TIP]
> Follow these recommendations for production Azure Pipelines.

---

## 📄 Use YAML Pipelines Instead of Classic Pipelines

Recommended:

```text
YAML Pipeline
```

Advantages:

* Stored with application code.
* Version controlled.
* Portable between projects.
* Easier to reuse.

---

## ♻️ Use Pipeline Templates

Avoid duplication:

```text
pipelines/
 |
 ├── templates/
 │       ├── build.yml
 │       └── deploy.yml
 |
 └── azure-pipelines.yml
```

Benefits:

* Reusable pipeline logic.
* Cleaner configuration.
* Easier maintenance.

---

## 🔐 Configure Environment Approvals

For production deployments:

```text
Build
  |
  ↓
Development
  |
  ↓
Approval Gate
  |
  ↓
Production
```

Benefits:

* Prevents accidental releases.
* Enables controlled deployments.

---

## ⚡ Enable Dependency Caching

Cache:

```text
node_modules
pip cache
```

Benefits:

* Faster pipeline execution.
* Reduced build time.
* Lower resource consumption.

Expected improvement:

```text
Up to 60% faster builds
```

---

# Validation Checklist

Verify:

* ✅ Pipeline created successfully.
* ✅ YAML file committed.
* ✅ Build stage completed.
* ✅ Tests executed.
* ✅ Test results published.
* ✅ Build artifact created.
* ✅ Deployment stage completed.
* ✅ Pipeline logs reviewed.

---

# Summary

In this lab, you:

* Created an Azure YAML CI/CD pipeline.
* Automated application build and testing.
* Published build artifacts.
* Added a deployment stage.
* Learned Azure Pipelines best practices.

This pipeline provides the foundation for enterprise CI/CD workflows with automated build, validation, and deployment processes.
