# Lab 23 — Working with Dockerfile

> [!IMPORTANT]
> This lab demonstrates how to create optimized Docker images using **Dockerfile best practices**, including **multi-stage builds**, **smaller production images**, **non-root execution**, and **container health checks**.

## Objective

Build optimized Dockerfiles using:

* 🏗️ Multi-stage builds.
* 🔐 Security best practices.
* 📦 Minimal production images.
* ❤️ Container health checks.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 18 — Working with Docker Images**.
* ✅ Docker Engine installed.
* ✅ Basic understanding of Dockerfile instructions.
* ✅ A Python Flask application (`app.py`).
* ✅ A `requirements.txt` file.

---

# Dockerfile Optimization Overview

A production Docker image should be:

* Small in size.
* Secure.
* Fast to download.
* Easy to maintain.
* Free from unnecessary build dependencies.

A multi-stage build separates:

```text id="6p9m2x"
Build Environment
        |
        ↓
Install Dependencies
        |
        ↓
Copy Required Files Only
        |
        ↓
Minimal Production Image
```

---

# Procedure

# Part 1 — Create Multi-Stage Dockerfile

## Step 1 — Builder Stage

The builder stage installs application dependencies.

```dockerfile id="4m8q7v"
# Stage 1: Builder

FROM python:3.11 AS builder

WORKDIR /build

COPY requirements.txt .

RUN pip install --user --no-cache-dir -r requirements.txt
```

Purpose:

* Uses a full Python image.
* Installs required dependencies.
* Prepares application packages.

---

## Step 2 — Production Stage

Create a minimal production image:

```dockerfile id="8q3n5m"
# Stage 2: Production image (minimal)

FROM python:3.11-slim AS production

WORKDIR /app
```

Benefits:

* Smaller base image.
* Reduced attack surface.
* Faster image downloads.

---

## Step 3 — Copy Required Packages

Copy only installed dependencies from the builder stage:

```dockerfile id="2v7m9x"
COPY --from=builder /root/.local /root/.local
```

This avoids copying unnecessary build files.

---

## Step 4 — Create Non-Root User

Create a dedicated application user:

```dockerfile id="9m4p6q"
RUN useradd -m -u 1000 appuser && chown -R appuser /app
```

Switch to the non-root user:

```dockerfile id="5x8n2v"
USER appuser
```

Security benefit:

* Prevents applications from running with root privileges.
* Reduces container security risks.

---

## Step 5 — Copy Application Code

Copy application files:

```dockerfile id="7q3m8p"
COPY --chown=appuser:appuser app.py .
```

---

## Step 6 — Configure Application Port

Expose application port:

```dockerfile id="1n6m9v"
EXPOSE 5000
```

---

## Step 7 — Configure Environment PATH

Add installed packages to PATH:

```dockerfile id="4p8m3q"
ENV PATH=/root/.local/bin:$PATH
```

---

## Step 8 — Add Container Health Check

Configure Docker health monitoring:

```dockerfile id="8m5q2x"
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
CMD curl -f http://localhost:5000/health || exit 1
```

Health checks allow:

* Docker.
* ECS.
* Kubernetes.

to monitor container availability.

---

## Step 9 — Define Application Startup Command

Start the Flask application:

```dockerfile id="6v2m9q"
CMD ["python", "app.py"]
```

---

# Complete Multi-Stage Dockerfile Example

```dockerfile id="3q7m5x"
# Stage 1: Builder

FROM python:3.11 AS builder

WORKDIR /build

COPY requirements.txt .

RUN pip install --user --no-cache-dir -r requirements.txt


# Stage 2: Production image

FROM python:3.11-slim AS production

WORKDIR /app

COPY --from=builder /root/.local /root/.local

RUN useradd -m -u 1000 appuser && chown -R appuser /app

USER appuser

COPY --chown=appuser:appuser app.py .

EXPOSE 5000

ENV PATH=/root/.local/bin:$PATH

HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
CMD curl -f http://localhost:5000/health || exit 1

CMD ["python", "app.py"]
```

---

# Part 2 — Build Production Image

## Step 10 — Build Using Production Target

Build only the production stage:

```bash id="5m8q7p"
docker build --target production -t myapp:prod .
```

The `--target` option selects the required build stage.

---

## Step 11 — Compare Image Sizes

View image size:

```bash id="9x4m2v"
docker images myapp
```

Compare:

* Builder image size.
* Production image size.

Expected result:

Production image should be significantly smaller.

---

# Docker Multi-Stage Build Workflow

```text id="7p2m8q"
Developer Code
       |
       ↓
Builder Stage
       |
       ↓
Install Dependencies
       |
       ↓
Production Stage
       |
       ↓
Minimal Secure Image
       |
       ↓
Deploy Container
```

---

# Dockerfile Best Practices

| Practice           | Benefit                      |
| ------------------ | ---------------------------- |
| Multi-stage builds | Smaller images               |
| Non-root USER      | Improved security            |
| Health checks      | Better orchestration support |
| .dockerignore      | Faster builds                |
| Versioned images   | Reproducible deployments     |

---

# Best Practice Tips

> [!TIP]
> Apply these Dockerfile standards when preparing images for production environments.

## 📦 Use Multi-Stage Builds

Production images can be:

```text id="2q9m5v"
60-80% smaller
```

Benefits:

* Faster ECR uploads.
* Faster container pulls.
* Reduced storage usage.

---

## 🔐 Always Run Containers as Non-Root

Use:

```dockerfile id="6m3q8p"
USER appuser
```

Benefits:

* Reduces privilege escalation risk.
* Meets security requirements in many organizations.
* Improves container isolation.

---

## ❤️ Use HEALTHCHECK

Include:

```dockerfile id="8v5m2x"
HEALTHCHECK
```

Benefits:

* ECS uses health checks.
* Kubernetes uses health checks.
* Improves automated recovery.

---

## 🚫 Use .dockerignore

Create:

```text id="3n7q5m"
.dockerignore
```

Exclude unnecessary files:

```text id="4x9m6v"
.git
__pycache__
*.pyc
node_modules
.env
```

Benefits:

* Smaller build context.
* Faster builds.
* Prevents accidental secret exposure.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Multi-stage Dockerfile created.
* ✅ Builder stage configured.
* ✅ Production stage configured.
* ✅ Dependencies copied from builder.
* ✅ Non-root user configured.
* ✅ Health check added.
* ✅ Production image built successfully.
* ✅ Image size compared.

---

# Summary

In this lab, you:

* Created an optimized multi-stage Dockerfile.
* Reduced image size using production builds.
* Implemented non-root container execution.
* Added health monitoring.
* Applied Docker image security practices.
* Prepared containers for production deployment.

You are now ready to continue with the next lab in the **Docker** learning path.
