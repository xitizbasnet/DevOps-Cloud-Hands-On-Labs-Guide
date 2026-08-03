# Project 7: Deploy Amazon-Like Shopping Application with Full DevOps Toolchain

## Overview

This project demonstrates the deployment of an **Amazon-like shopping application** using a complete **DevOps automation toolchain**.

The solution integrates continuous integration, security scanning, containerization, container image management, and cloud deployment using:

* Terraform for infrastructure automation.
* Jenkins for CI/CD orchestration.
* SonarQube for code quality analysis.
* Trivy for container vulnerability scanning.
* Docker for application containerization.
* Amazon Elastic Container Registry (ECR) for image storage.
* Amazon Elastic Container Service (ECS) for application deployment.

This implementation follows modern DevOps practices by automating the complete software delivery lifecycle.

---

# 🛠️ Tools and Technologies

| Tool / Technology                          | Purpose                                                                  |
| ------------------------------------------ | ------------------------------------------------------------------------ |
| 🏗️ Terraform                              | Automates AWS infrastructure provisioning                                |
| 🧩 Jenkins CI/CD                           | Automates application build, testing, scanning, and deployment workflows |
| 🔍 SonarQube                               | Performs source code quality analysis                                    |
| 🐳 Docker                                  | Builds and packages applications as containers                           |
| 🛡️ Trivy                                  | Scans container images for security vulnerabilities                      |
| 📦 Amazon Elastic Container Registry (ECR) | Stores private Docker container images                                   |
| 🚀 Amazon Elastic Container Service (ECS)  | Runs and manages containerized applications                              |

---

# 🏛️ Architecture Overview

The application deployment workflow follows this DevOps pipeline:

```text
Code Push
    |
    ▼
Jenkins Pipeline
    |
    ▼
SonarQube
(Code Quality Analysis)
    |
    ▼
Quality Gate Validation
    |
    ▼
Trivy
(Container Security Scan)
    |
    ▼
Docker Build
    |
    ▼
Amazon ECR Push
(Container Image Storage)
    |
    ▼
Amazon ECS Deployment
(Application Hosting)
```

---

# 🚀 End-to-End Deployment Workflow

The Jenkins pipeline automates the following stages:

1. Developer pushes application code.
2. Jenkins triggers the CI/CD pipeline.
3. SonarQube analyzes source code quality.
4. Quality Gate validates code standards.
5. Docker builds the application container image.
6. Trivy scans the image for vulnerabilities.
7. Container image is pushed to Amazon ECR.
8. ECS service is updated with the latest deployment.

---

# 📄 Jenkins Pipeline Configuration

## Jenkinsfile — Complete Pipeline

```groovy id="6qz8r1"
pipeline {

    agent any

    stages {


        stage('Code Quality - SonarQube') {

            steps {

                withSonarQubeEnv('sonar-server') {

                    sh 'mvn sonar:sonar -Dsonar.projectKey=shopping-app'

                }

            }

        }


        stage('Quality Gate') {

            steps {

                waitForQualityGate abortPipeline: true

            }

        }


        stage('Docker Build') {

            steps {

                sh 'docker build -t shopping-app:${BUILD_NUMBER} .'

            }

        }


        stage('Trivy Scan') {

            steps {

                sh 'trivy image --exit-code 1 --severity HIGH,CRITICAL shopping-app:${BUILD_NUMBER}'

            }

        }


        stage('Push to ECR') {

            steps {

                // ECR login and push

            }

        }


        stage('Deploy to ECS') {

            steps {

                sh 'aws ecs update-service --cluster prod-cluster --service shopping-app --force-new-deployment'

            }

        }

    }

}
```

---

# 🔄 Pipeline Stage Details

## Stage 1: Code Quality Analysis - SonarQube

### Purpose

Analyzes application source code to identify quality issues and maintain coding standards.

### Actions Performed

* Connects Jenkins with SonarQube server.
* Runs Maven-based SonarQube analysis.
* Publishes code quality metrics.

Command:

```bash
mvn sonar:sonar -Dsonar.projectKey=shopping-app
```

---

# Stage 2: Quality Gate Validation

## Purpose

Ensures the application meets predefined quality standards before continuing.

Configuration:

```groovy
waitForQualityGate abortPipeline: true
```

Benefits:

* Prevents poor-quality code from moving forward.
* Stops pipeline execution when quality requirements fail.
* Enforces development standards.

> [!IMPORTANT]
> Production deployments should only continue after passing the SonarQube Quality Gate.

---

# Stage 3: Docker Image Build

## Purpose

Creates a deployable container image for the shopping application.

Command:

```bash
docker build -t shopping-app:${BUILD_NUMBER} .
```

Example image:

```text
shopping-app:25
```

Benefits:

* Provides consistent runtime environments.
* Simplifies application deployment.
* Enables container-based scaling.

---

# Stage 4: Trivy Container Security Scan

## Purpose

Scans Docker images for known security vulnerabilities.

Command:

```bash
trivy image --exit-code 1 --severity HIGH,CRITICAL shopping-app:${BUILD_NUMBER}
```

Scan behavior:

| Severity Level | Pipeline Action |
| -------------- | --------------- |
| HIGH           | Pipeline fails  |
| CRITICAL       | Pipeline fails  |

> [!WARNING]
> Container images containing high or critical vulnerabilities should not be deployed to production environments.

---

# Stage 5: Push Docker Image to Amazon ECR

## Purpose

Stores the validated container image in Amazon Elastic Container Registry.

Pipeline actions:

* Authenticates with Amazon ECR.
* Tags the Docker image.
* Pushes the image repository.

Example workflow:

```text
Docker Image
      |
      ▼
Amazon ECR Repository
```

---

# Stage 6: Deploy Application to Amazon ECS

## Purpose

Updates the ECS service with the latest application version.

Command:

```bash
aws ecs update-service \
--cluster prod-cluster \
--service shopping-app \
--force-new-deployment
```

Actions performed:

* Triggers ECS deployment.
* Starts new application containers.
* Replaces previous running tasks.

---

# 🔐 Security and Quality Controls

The pipeline includes multiple validation layers:

| Security / Quality Layer | Tool                 | Purpose                         |
| ------------------------ | -------------------- | ------------------------------- |
| Code Analysis            | SonarQube            | Detects code quality issues     |
| Quality Approval         | Jenkins Quality Gate | Prevents low-quality releases   |
| Image Security           | Trivy                | Finds container vulnerabilities |
| Image Management         | Amazon ECR           | Secure container storage        |
| Deployment Control       | ECS                  | Managed container hosting       |

---

# ✅ Best Practice Recommendations

## Automate Infrastructure with Terraform

Use Terraform to create and manage AWS resources.

Benefits:

* Repeatable infrastructure.
* Version-controlled changes.
* Faster environment creation.

---

## Enforce Code Quality Gates

Require SonarQube approval before deployment.

Benefits:

* Improves code reliability.
* Reduces production defects.
* Maintains coding standards.

---

## Scan Images Before Deployment

Use Trivy in every container build pipeline.

Benefits:

* Detects vulnerabilities early.
* Prevents insecure images from reaching production.
* Improves application security.

---

## Use Immutable Container Images

Tag images using unique identifiers.

Example:

```text
shopping-app:${BUILD_NUMBER}
```

Recommended production approach:

```text
shopping-app@sha256:<image-digest>
```

Benefits:

* Guarantees exact image versions.
* Improves deployment tracking.
* Prevents unexpected image changes.

---

## Enable ECS Deployment Monitoring

Monitor ECS service deployments.

Benefits:

* Detects failed deployments.
* Improves application availability.
* Supports automated recovery processes.

---

# 📚 Summary

This project implements a complete DevOps delivery pipeline for an Amazon-like shopping application using:

* 🏗️ Terraform infrastructure automation.
* 🧩 Jenkins CI/CD orchestration.
* 🔍 SonarQube code quality validation.
* 🛡️ Trivy container security scanning.
* 🐳 Docker containerization.
* 📦 Amazon ECR image management.
* 🚀 Amazon ECS application deployment.

The complete workflow enables secure, automated, and production-ready application delivery following modern DevOps engineering practices.
