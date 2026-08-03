# Project 4: Deploy Application on AKS Using Jenkins CI/CD

## Overview

This project demonstrates an automated **CI/CD deployment pipeline** using **Jenkins**, **Docker**, **Azure Container Registry (ACR)**, **Azure Kubernetes Service (AKS)**, and **kubectl**.

The pipeline automates the complete application delivery process:

* Building the application container image.
* Running application tests.
* Publishing container images to Azure Container Registry.
* Deploying updated images to Azure Kubernetes Service.
* Validating successful Kubernetes rollout.

---

# 🛠️ Tools and Technologies

| Tool / Technology                 | Purpose                                                |
| --------------------------------- | ------------------------------------------------------ |
| 🧩 Jenkins                        | Automates CI/CD pipeline execution                     |
| 🐳 Docker                         | Builds and packages application containers             |
| 📦 Azure Container Registry (ACR) | Stores private Docker container images                 |
| ☸️ Azure Kubernetes Service (AKS) | Hosts and manages containerized applications           |
| ⚙️ kubectl                        | Command-line tool for Kubernetes deployment management |

---

# 🚀 Deployment Workflow

The Jenkins pipeline performs the following stages:

1. **Build & Test**

   * Builds the Docker image.
   * Runs application tests inside the container.

2. **Push to Azure Container Registry**

   * Authenticates with ACR.
   * Pushes the container image.

3. **Deploy to AKS**

   * Connects Jenkins to the AKS cluster.
   * Updates the Kubernetes deployment image.
   * Waits for successful rollout completion.

---

# 📄 Jenkins Pipeline Configuration

## Jenkinsfile

```groovy id="q8n5j3"
pipeline {

    agent any

    environment {

        ACR_NAME = 'devopsacr'
        IMAGE_NAME = 'devops-app'
        AKS_CLUSTER = 'devops-aks'
        RESOURCE_GROUP = 'DevOps-RG'

    }

    stages {

        stage('Build & Test') {

            steps {

                sh 'docker build -t ${ACR_NAME}.azurecr.io/${IMAGE_NAME}:${BUILD_NUMBER} .'

                sh 'docker run --rm ${ACR_NAME}.azurecr.io/${IMAGE_NAME}:${BUILD_NUMBER} npm test'

            }

        }


        stage('Push to ACR') {

            steps {

                withCredentials([string(credentialsId: 'acr-password', variable: 'ACR_PASS')]) {

                    sh 'docker login ${ACR_NAME}.azurecr.io -u ${ACR_NAME} -p $ACR_PASS'

                    sh 'docker push ${ACR_NAME}.azurecr.io/${IMAGE_NAME}:${BUILD_NUMBER}'

                }

            }

        }


        stage('Deploy to AKS') {

            steps {

                sh 'az aks get-credentials -g ${RESOURCE_GROUP} -n ${AKS_CLUSTER} --overwrite-existing'

                sh 'kubectl set image deployment/myapp myapp=${ACR_NAME}.azurecr.io/${IMAGE_NAME}:${BUILD_NUMBER}'

                sh 'kubectl rollout status deployment/myapp'

            }

        }

    }

}
```

---

# 🔄 Pipeline Stage Details

## Stage 1: Build & Test

### Purpose

Creates the application container image and validates application functionality before deployment.

### Actions Performed

* Builds Docker image using the application source code.
* Tags the image with the Jenkins build number.
* Runs automated application tests inside the container.

Example image format:

```text
devopsacr.azurecr.io/devops-app:<BUILD_NUMBER>
```

---

## Stage 2: Push to Azure Container Registry (ACR)

### Purpose

Stores the validated Docker image in Azure Container Registry.

### Actions Performed

* Retrieves ACR credentials from Jenkins Credentials Store.
* Authenticates with Azure Container Registry.
* Pushes the Docker image.

> [!IMPORTANT]
> Container registry credentials must never be stored directly inside the Jenkinsfile.

---

## Stage 3: Deploy to Azure Kubernetes Service (AKS)

### Purpose

Updates the running Kubernetes application with the newly built container image.

### Actions Performed

* Retrieves AKS cluster credentials.
* Updates the Kubernetes deployment image.
* Monitors deployment rollout status.

Commands used:

```bash
az aks get-credentials
```

Connects Jenkins to the AKS cluster.

```bash
kubectl set image deployment/myapp
```

Updates the running application image.

```bash
kubectl rollout status deployment/myapp
```

Confirms successful deployment completion.

---

# 🔐 Credential Management

Jenkins credentials are managed using the built-in **Jenkins Credentials Store**.

Configured credential:

| Credential     | Purpose                                                    |
| -------------- | ---------------------------------------------------------- |
| `acr-password` | Provides secure authentication to Azure Container Registry |

> [!WARNING]
> Never store passwords, access keys, or secrets directly inside Jenkins pipeline files.

---

# ✅ Best Practice Tips

## Store ACR Credentials in Jenkins Credentials Store

Use Jenkins credential management instead of hardcoding credentials.

Benefits:

* Protects sensitive information.
* Enables secure credential rotation.
* Prevents accidental credential exposure.

---

## Use `kubectl rollout status`

Use rollout monitoring to ensure deployment success.

Benefits:

* Pipeline waits for Kubernetes deployment completion.
* Detects failed deployments automatically.
* Prevents false-positive successful builds.

Example:

```bash
kubectl rollout status deployment/myapp
```

---

## Add Automatic Rollback Handling

Add a Jenkins `post` failure block to handle unsuccessful deployments.

Example:

```groovy
post {

    failure {

        // Execute rollback process

    }

}
```

Benefits:

* Automatically responds to deployment failures.
* Improves application availability.
* Reduces manual recovery steps.

---

## Use Immutable Image References

Use image digests instead of build numbers for production deployments.

Example:

```text
devopsacr.azurecr.io/devops-app@sha256:<digest>
```

Benefits:

* Guarantees deployment of the exact image version.
* Prevents unexpected image changes.
* Improves deployment traceability.

---

# 📚 Summary

This Jenkins-based AKS CI/CD pipeline provides an automated and reliable deployment workflow by combining:

* 🧩 Jenkins pipeline automation.
* 🐳 Docker container builds.
* 📦 Azure Container Registry image management.
* ☸️ AKS Kubernetes deployments.
* ⚙️ kubectl-based rollout validation.

Following these practices enables secure, repeatable, and production-ready container application deployments on Azure.
