# Lab 18 — Working with Docker Images

> [!IMPORTANT]
> This lab introduces Docker image management, including pulling images, inspecting image layers, tagging images, building custom images, and maintaining image storage.

## Objective

Pull, inspect, tag, build custom images, and manage image layers.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 16 — Install & Configure Docker on Ubuntu Server**.
* ✅ Docker Engine installed and running.
* ✅ Basic understanding of containers and Docker commands.
* ✅ SSH access to your Ubuntu server.

---

# Procedure

## Step 1 — Pull Docker Images

Download official Docker images:

### Pull Ubuntu Image

```bash id="8m5q2v"
docker pull ubuntu:22.04
```

### Pull Python Image

```bash id="4n7x9m"
docker pull python:3.11-slim
```

Pulled images are stored locally in the Docker image cache.

---

## Step 2 — List Available Docker Images

Display available images:

```bash id="6p3m8q"
docker images
```

Alternative formatted output:

```bash id="1v9k4x"
docker image ls --format "table {{.Repository}} {{.Tag}} {{.Size}}"
```

Example output:

```text
REPOSITORY     TAG          SIZE
ubuntu         22.04        xxx MB
python         3.11-slim    xxx MB
```

---

## Step 3 — Inspect Image Layers

View the layer history of an image:

```bash id="7m2q5n"
docker history nginx --no-trunc
```

Image layers show:

* Base image layers.
* Commands executed during image creation.
* Layer sizes.
* Build history.

---

## Step 4 — Tag a Docker Image

Create a custom tag:

```bash id="3x8m6p"
docker tag nginx:latest my-nginx:v1.0
```

Verify the tag:

```bash id="5q9n2m"
docker images
```

Tagging helps identify:

* Application versions.
* Deployment releases.
* Environment-specific images.

---

# Step 5 — Build a Custom Docker Image

Create an application directory:

```bash id="9m4p7x"
mkdir ~/myapp
cd ~/myapp
```

---

## Step 5.1 — Create Dockerfile

Create a Dockerfile:

```bash id="2k6v8m"
cat > Dockerfile << 'EOF'
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

EXPOSE 5000

CMD ["python", "app.py"]
EOF
```

---

## Dockerfile Explanation

| Instruction | Purpose                            |
| ----------- | ---------------------------------- |
| `FROM`      | Defines the base image             |
| `WORKDIR`   | Sets application working directory |
| `COPY`      | Copies files into image            |
| `RUN`       | Executes build commands            |
| `EXPOSE`    | Documents application port         |
| `CMD`       | Defines container startup command  |

---

# Step 6 — Build Docker Image

Build the image:

```bash id="8x4m2q"
docker build -t myapp:1.0 .
```

Build without using cache:

```bash id="6n9p3v"
docker build -t myapp:1.0 --no-cache .
```

The `--no-cache` option forces a completely fresh image build.

---

## Step 7 — Remove Unused Images

Clean unused Docker images:

```bash id="4p7m9x"
docker image prune -f
```

This removes unused image layers and frees disk space.

---

# Docker Image Workflow

The Docker image lifecycle follows this process:

```text id="7q3m8p"
Docker Registry
       |
       ↓
docker pull
       |
       ↓
Local Image Store
       |
       ↓
Tag Image
       |
       ↓
Build Custom Image
       |
       ↓
Run Container
```

---

# Understanding Docker Image Layers

Docker images are built using layers.

Example:

```text id="9v5m2k"
Base Image Layer
        |
        ↓
Operating System Layer
        |
        ↓
Application Dependencies
        |
        ↓
Application Code
```

Benefits of layers:

* Faster builds.
* Reusable components.
* Efficient storage.

---

# Best Practice Tips

> [!TIP]
> Follow these practices to create secure, efficient, and reproducible Docker images.

## 📌 Pin Exact Image Versions

Avoid:

```dockerfile
FROM python:latest
```

Use:

```dockerfile
FROM python:3.11.4-slim
```

Benefits:

* Reproducible builds.
* Predictable deployments.
* Reduced unexpected changes.

---

## 🏗️ Use Multi-Stage Builds

Use multi-stage Docker builds to:

* Reduce final image size.
* Remove unnecessary build dependencies.
* Improve security.

> Multi-stage builds are covered in **Lab 23**.

---

## 🔍 Scan Images for Vulnerabilities

Scan images using:

Docker Scout:

```bash id="2q7m8x"
docker scout cves myapp:1.0
```

Or Trivy:

```bash id="5m3n9v"
trivy image myapp:1.0
```

Benefits:

* Detect known vulnerabilities.
* Improve container security.
* Support compliance requirements.

---

## ⚡ Optimize Docker Layer Cache

Follow the correct Dockerfile order:

Recommended:

```dockerfile
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY .
```

Why:

* Dependency installation runs only when requirements change.
* Faster rebuilds.
* Better cache utilization.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Docker images pulled successfully.
* ✅ Local images listed.
* ✅ Image layers inspected.
* ✅ Image tagged successfully.
* ✅ Custom Dockerfile created.
* ✅ Custom image built successfully.
* ✅ Unused images cleaned.

---

# Summary

In this lab, you:

* Pulled official Docker images.
* Inspected Docker image layers.
* Created custom image tags.
* Built a custom Python application image.
* Learned Dockerfile structure.
* Applied image optimization and security practices.

You are now ready to continue with the next lab in the **Docker** learning path.
