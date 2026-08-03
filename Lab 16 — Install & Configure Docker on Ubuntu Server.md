# Section 4 — Docker

## Lab 16 — Install & Configure Docker on Ubuntu Server

> [!IMPORTANT]
> This lab demonstrates how to install **Docker Engine** on an Ubuntu EC2 server, configure Docker access, and run your first container.

## Objective

Install Docker Engine on Ubuntu EC2 and run the first container.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ An Ubuntu EC2 instance.
* ✅ SSH access to the server.
* ✅ A user account with `sudo` privileges.
* ✅ Basic Linux command-line knowledge.

---

# Procedure

## Step 1 — Update System and Install Docker Dependencies

Update Ubuntu packages:

```bash id="9m3v7q"
sudo apt update && sudo apt upgrade -y
```

Install required packages:

```bash id="4x8n2k"
sudo apt install -y ca-certificates curl gnupg lsb-release
```

---

## Step 2 — Add Docker Official GPG Key

Create the Docker key directory:

```bash id="7p2m5v"
sudo mkdir -p /etc/apt/keyrings
```

Download and add the Docker GPG key:

```bash id="3n8q6w"
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

---

## Step 3 — Add Docker Repository

Add the official Docker repository:

```bash id="5v9k3m"
echo "deb [arch=$(dpkg --print-architecture) \
signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Update package information:

```bash id="8q4m6p"
sudo apt update
```

---

## Step 4 — Install Docker Engine

Install Docker components:

```bash id="1x7m9q"
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

Installed components:

| Component             | Purpose                                |
| --------------------- | -------------------------------------- |
| Docker Engine         | Container runtime                      |
| Docker CLI            | Command-line management tool           |
| containerd            | Container lifecycle management         |
| Docker Compose Plugin | Multi-container application management |

---

# Step 5 — Start and Enable Docker Service

Start Docker:

```bash id="6m3p8v"
sudo systemctl start docker
```

Enable Docker at system startup:

```bash id="4q9n5x"
sudo systemctl enable docker
```

---

## Step 6 — Verify Docker Installation

Check Docker version:

```bash id="7k2m4s"
docker --version
```

View Docker system information:

```bash id="0v5n8m"
docker info
```

---

# Step 7 — Configure Docker User Access

Add the current Ubuntu user to the Docker group:

```bash id="9r4p6x"
sudo usermod -aG docker $USER
```

Reload group membership:

```bash id="2m8q5v"
newgrp docker
```

---

## Step 8 — Run Your First Docker Container

Run the Docker test container:

```bash id="5n7m3k"
docker run hello-world
```

Expected result:

```text id="4x8q9m"
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

---

# Docker Installation Workflow

The Docker setup process follows this workflow:

```text id="6q3m8p"
Ubuntu EC2 Server
        |
        ↓
Install Docker Dependencies
        |
        ↓
Add Docker Repository
        |
        ↓
Install Docker Engine
        |
        ↓
Enable Docker Service
        |
        ↓
Configure User Access
        |
        ↓
Run First Container
```

---

# Understanding Docker Components

## 🐳 Docker Engine

Responsible for:

* Creating containers.
* Running container workloads.
* Managing images and networks.

---

## 📦 Docker Container

A lightweight, isolated runtime environment containing:

* Application code.
* Dependencies.
* Required libraries.

---

## 🖼️ Docker Image

A reusable template used to create containers.

Example:

```bash id="8v2m5q"
docker run nginx
```

Docker downloads the image and creates a running container.

---

# Best Practice Tips

> [!TIP]
> Follow Docker security and maintenance practices when managing long-running environments.

## 🔐 Avoid Running Docker as Root

Never run Docker as root in production.

Recommended approaches:

* Use rootless Docker.
* Use dedicated Docker users.
* Apply least-privilege permissions.

---

## 🧹 Clean Unused Docker Resources

Regularly remove unused resources:

```bash id="3p7m9x"
docker system prune -a
```

Benefits:

* Frees disk space.
* Removes unused images.
* Removes stopped containers.

---

## 🔒 Secure Docker Remote Access

If exposing the Docker socket remotely:

Enable:

```text id="5q8n2m"
Docker daemon TLS
```

Purpose:

* Prevent unauthorized Docker access.
* Reduce risk of container breakout.

---

## 📋 Configure Container Log Limits

Set Docker log rotation limits:

```bash id="1m6q9v"
--log-opt max-size=10m --log-opt max-file=3
```

Benefits:

* Prevents unlimited log growth.
* Controls disk usage.
* Improves server stability.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Docker repository configured.
* ✅ Docker Engine installed.
* ✅ Docker service started.
* ✅ Docker enabled at boot.
* ✅ Docker version verified.
* ✅ User added to Docker group.
* ✅ First Docker container executed successfully.

---

# Summary

In this lab, you:

* Installed Docker Engine on Ubuntu EC2.
* Configured the official Docker repository.
* Started and enabled Docker services.
* Added a user to the Docker group.
* Ran your first Docker container.
* Learned Docker security and maintenance best practices.

You are now ready to continue with the next lab in the **Docker** learning path.
