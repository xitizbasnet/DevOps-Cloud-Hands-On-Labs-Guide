# Lab 26 — Deploy & Test Application on Kubernetes Cluster

> [!IMPORTANT]
> This lab demonstrates how to deploy an application on Kubernetes, expose it externally using a **NodePort Service**, scale application replicas, perform rolling updates, and safely roll back deployments.

## Objective

Deploy and manage an Nginx application by performing:

* 🚀 Kubernetes Deployment creation.
* 🌐 Service exposure using NodePort.
* 📈 Application scaling.
* 🔄 Rolling updates.
* ↩️ Deployment rollback.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 25 — Install & Configure 3-Node Kubernetes Cluster with Kubeadm**.
* ✅ A running Kubernetes cluster.
* ✅ `kubectl` configured on the master node.
* ✅ Worker nodes showing `Ready` status.

Verify cluster:

```bash id="7m4q9p"
kubectl get nodes
```

Expected:

```text id="8x2m5v"
NAME            STATUS
k8s-master      Ready
k8s-worker-1    Ready
k8s-worker-2    Ready
```

---

# Kubernetes Application Deployment Overview

Application flow:

```text id="3q8m6v"
User
 |
 ↓
NodePort Service
 |
 ↓
Kubernetes Service
 |
 ↓
Nginx Pods
 |
 ↓
Worker Nodes
```

---

# Procedure

# Part 1 — Create Kubernetes Deployment

## Step 1 — Create Deployment YAML File

Create:

```bash id="9p5m2v"
nano nginx-deploy.yaml
```

Add the following configuration:

```yaml id="2v7m9q"
# nginx-deploy.yaml

apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx-app
  labels:
    app: nginx

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx:1.25

        ports:
        - containerPort: 80

        resources:
          requests:
            memory: "64Mi"
            cpu: "100m"

          limits:
            memory: "128Mi"
            cpu: "200m"


---

apiVersion: v1
kind: Service

metadata:
  name: nginx-service

spec:
  type: NodePort

  selector:
    app: nginx

  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080
```

---

# Deployment Configuration Explanation

## Deployment Settings

| Configuration  | Value         | Purpose                                 |
| -------------- | ------------- | --------------------------------------- |
| Replicas       | 3             | Runs three Nginx pods                   |
| Strategy       | RollingUpdate | Zero/minimal downtime updates           |
| maxSurge       | 1             | Allows one additional pod during update |
| maxUnavailable | 0             | Keeps all pods available                |

---

## Container Resource Settings

Resource requests:

```yaml
requests:
  memory: "64Mi"
  cpu: "100m"
```

Defines minimum resources required.

Resource limits:

```yaml
limits:
  memory: "128Mi"
  cpu: "200m"
```

Defines maximum resource consumption.

---

# Part 2 — Deploy Application

## Step 2 — Apply Kubernetes Manifest

Deploy the application:

```bash id="6q2m8v"
kubectl apply -f nginx-deploy.yaml
```

Expected:

```text
deployment.apps/nginx-app created
service/nginx-service created
```

---

## Step 3 — Verify Pods

Check running pods:

```bash id="4n7m9x"
kubectl get pods -o wide
```

Example:

```text
NAME                         STATUS
nginx-app-xxxxxxxxxx         Running
nginx-app-xxxxxxxxxx         Running
nginx-app-xxxxxxxxxx         Running
```

---

# Part 3 — Expose Application Using NodePort

## Step 4 — Check Service

View service details:

```bash id="8m5q2x"
kubectl get svc nginx-service
```

Example:

```text
NAME             TYPE       PORT
nginx-service    NodePort   80:30080
```

---

## Step 5 — Access Application

Open browser:

```text id="5v9m3q"
http://<WORKER-IP>:30080
```

Replace:

```text id="1m8p5x"
<WORKER-IP>
```

with the public or accessible worker node IP.

Expected result:

```text
Welcome to nginx!
```

---

# Part 4 — Scale Application

## Step 6 — Increase Replica Count

Scale deployment:

```bash id="7q3m8p"
kubectl scale deployment nginx-app --replicas=5
```

---

## Step 7 — Verify Additional Pods

Check pods:

```bash id="9m4p6q"
kubectl get pods
```

Expected:

```text
5 nginx pods running
```

---

# Part 5 — Perform Rolling Update

## Step 8 — Update Nginx Image Version

Update image:

```bash id="2x8m5q"
kubectl set image deployment/nginx-app nginx=nginx:1.26
```

Kubernetes will:

* Create new pods.
* Remove old pods gradually.
* Maintain application availability.

---

## Step 9 — Monitor Rollout Status

Check update progress:

```bash id="6p9m2v"
kubectl rollout status deployment/nginx-app
```

---

## Step 10 — View Deployment History

View previous versions:

```bash id="3m7q8p"
kubectl rollout history deployment/nginx-app
```

---

# Part 6 — Rollback Deployment

## Step 11 — Undo Latest Deployment

Rollback:

```bash id="8v5m2x"
kubectl rollout undo deployment/nginx-app
```

Verify:

```bash id="4q9m6v"
kubectl rollout status deployment/nginx-app
```

---

# Kubernetes Deployment Lifecycle

```text id="5m2q8v"
Create YAML
     |
     ↓
kubectl apply
     |
     ↓
Pods Created
     |
     ↓
Expose Service
     |
     ↓
Scale Application
     |
     ↓
Rolling Update
     |
     ↓
Rollback if Required
```

---

# Useful Kubernetes Commands

| Command                      | Purpose                 |
| ---------------------------- | ----------------------- |
| `kubectl apply -f file.yaml` | Create/update resources |
| `kubectl get pods`           | View pods               |
| `kubectl get svc`            | View services           |
| `kubectl scale`              | Change replica count    |
| `kubectl set image`          | Update container image  |
| `kubectl rollout status`     | Monitor deployment      |
| `kubectl rollout history`    | View versions           |
| `kubectl rollout undo`       | Roll back deployment    |

---

# Best Practice Tips

> [!TIP]
> Follow these recommendations when deploying applications in production Kubernetes environments.

## 📊 Always Define Resource Requests and Limits

Use:

```yaml
resources:
  requests:
  limits:
```

Benefits:

* Prevents resource starvation.
* Improves cluster scheduling.
* Protects workloads from excessive usage.

---

## 🔄 Use RollingUpdate Strategy

Recommended:

```yaml
strategy:
  type: RollingUpdate
```

With:

```yaml
maxUnavailable: 0
```

Benefits:

* Zero-downtime deployments.
* Continuous application availability.

---

## 🗂️ Use Namespaces for Environment Isolation

Create separate environments:

```bash id="7x3m9q"
kubectl create ns dev
```

```bash id="8m5q2v"
kubectl create ns staging
```

```bash id="1q6m8p"
kubectl create ns prod
```

Benefits:

* Better resource organization.
* Safer deployments.
* Environment separation.

---

## ❤️ Add Liveness and Readiness Probes

Production deployments should include:

```yaml
livenessProbe:
readinessProbe:
```

Benefits:

* Automatic failure detection.
* Safer traffic routing.
* Better application reliability.

---

# Validation Checklist

Verify:

* ✅ Nginx deployment created.
* ✅ Three replicas running.
* ✅ NodePort service created.
* ✅ Application accessible externally.
* ✅ Deployment scaled successfully.
* ✅ Rolling update completed.
* ✅ Rollback tested.
* ✅ Resource limits configured.

---

# Summary

In this lab, you:

* Created a Kubernetes Deployment.
* Exposed an application using NodePort.
* Managed application replicas.
* Performed rolling updates.
* Tested deployment rollback.
* Applied Kubernetes production deployment practices.

You are now ready to continue with advanced Kubernetes topics such as configuration management, storage, and orchestration patterns.
