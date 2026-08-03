# Lab 21 — Docker Networking

> [!IMPORTANT]
> This lab introduces Docker networking concepts and demonstrates how to create and manage different network types, connect containers, and enable container-to-container communication.

## Objective

Create and manage:

* 🌉 Bridge networks.
* 🖥️ Host networks.
* 🔒 None networks.
* 🌐 Overlay networks.

Connect containers and understand Docker networking behavior.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 17 — Working with Containers**.
* ✅ Docker Engine installed and running.
* ✅ Basic understanding of Docker containers.
* ✅ SSH access to your Ubuntu server.

---

# Docker Networking Overview

Docker provides different networking drivers:

| Network Type | Description                        | Use Case                                     |
| ------------ | ---------------------------------- | -------------------------------------------- |
| Bridge       | Default isolated container network | Single-host container communication          |
| Host         | Shares host network stack          | High-performance workloads                   |
| None         | Completely isolated network        | Security testing                             |
| Overlay      | Multi-host container communication | Docker Swarm / Kubernetes-style environments |

---

# Procedure

# Part 1 — View Docker Networks

## Step 1 — List Available Networks

Display existing Docker networks:

```bash id="4m8q2v"
docker network ls
```

Example output:

```text id="8p3n6m"
NETWORK ID     NAME      DRIVER
xxxxxx         bridge    bridge
xxxxxx         host      host
xxxxxx         none      null
```

---

# Part 2 — Create Custom Bridge Network

## Step 2 — Create Docker Bridge Network

Create a custom network:

```bash id="7q5m3x"
docker network create --driver bridge devops-net
```

Custom bridge networks provide:

* Container isolation.
* Automatic DNS resolution.
* Container name-based communication.

---

## Step 3 — Run Containers on the Same Network

Start backend container:

```bash id="2v9m5q"
docker run -d --name backend \
--network devops-net \
nginx
```

Start frontend container:

```bash id="6x4m8p"
docker run -d --name frontend \
--network devops-net \
nginx
```

Both containers are now connected to:

```text id="5m7q2n"
devops-net
```

---

# Part 3 — Test Container DNS Resolution

## Step 4 — Communicate Between Containers

Access the frontend container:

```bash id="9n3q6v"
docker exec -it frontend bash
```

Test backend name resolution:

```bash id="1m8p5x"
ping -c3 backend
```

Docker automatically resolves:

```text id="7v2m9q"
backend → backend container IP address
```

---

# Part 4 — Inspect Docker Network

## Step 5 — View Network Details

Inspect the custom network:

```bash id="3q8m6v"
docker network inspect devops-net
```

Displays:

* Connected containers.
* IP addresses.
* Network configuration.
* Driver information.

---

# Part 5 — Connect Existing Containers

## Step 6 — Attach Existing Container to Network

Connect an existing container:

```bash id="8m4q7p"
docker network connect devops-net mycontainer
```

The container can now communicate through:

```text id="5x9m3q"
devops-net
```

---

# Part 6 — Host Network Mode

## Step 7 — Run Container Using Host Networking

Start container with host network:

```bash id="6q2m8v"
docker run -d --network host nginx
```

Characteristics:

* Container shares host networking.
* No port mapping required.
* Less network isolation.

---

# Part 7 — None Network Mode

## Step 8 — Run Fully Isolated Container

Create an isolated container:

```bash id="4n7m9x"
docker run -d --network none busybox sleep 1000
```

Characteristics:

* No external network access.
* No container communication.
* Used for isolated workloads.

---

# Part 8 — Cleanup

## Step 9 — Remove Docker Network

Delete custom network:

```bash id="9p5m2v"
docker network rm devops-net
```

---

# Docker Networking Workflow

The Docker networking workflow:

```text id="3m8q6v"
Create Network
       |
       ↓
Attach Containers
       |
       ↓
Container DNS Resolution
       |
       ↓
Application Communication
       |
       ↓
Monitor / Remove Network
```

---

# Network Driver Comparison

| Feature        | Bridge                | Host                  | None             | Overlay              |
| -------------- | --------------------- | --------------------- | ---------------- | -------------------- |
| Isolation      | ✅ Yes                 | ❌ No                  | ✅ Full           | ✅ Yes                |
| Multi-host     | ❌ No                  | ❌ No                  | ❌ No             | ✅ Yes                |
| DNS Resolution | ✅ Yes (custom bridge) | Host-based            | ❌ No             | ✅ Yes                |
| Performance    | Good                  | Highest               | N/A              | Good                 |
| Common Usage   | Single host apps      | High-performance apps | Security testing | Cluster environments |

---

# Best Practice Tips

> [!TIP]
> Design Docker networks based on application architecture and security requirements.

## 🌉 Use Custom Bridge Networks

Always prefer custom bridge networks:

```bash id="7m4q9p"
docker network create devops-net
```

Reasons:

* Supports automatic DNS resolution.
* Provides better isolation.
* Allows container name communication.

Avoid relying on the default bridge network because it does not support DNS-based container discovery.

---

## ⚡ Use Host Network Carefully

Use:

```bash id="2q8m5v"
--network host
```

only for:

* High-performance applications.
* Low-latency networking requirements.

Trade-off:

* Higher performance.
* Reduced network isolation.

---

## 🔐 Segment Application Networks

Recommended architecture:

```text id="8x3m6q"
Frontend
   |
   ↓
backend-net
   |
   ↓
Application Containers
   |
   ↓
Database Network
```

Example:

* Database containers → `backend-net`.
* Application containers → connect to required networks only.

Benefits:

* Improved security.
* Reduced unnecessary communication.
* Better isolation.

---

## 🌐 Use Overlay Networks for Multi-Host Systems

For distributed environments:

Use:

```text id="5v9m2x"
Overlay network driver
```

Common platforms:

* Docker Swarm.
* Kubernetes environments.

Purpose:

* Enable communication between containers running on different hosts.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Docker networks listed.
* ✅ Custom bridge network created.
* ✅ Containers connected to custom network.
* ✅ Container DNS resolution tested.
* ✅ Network details inspected.
* ✅ Existing container connected to network.
* ✅ Host networking tested.
* ✅ Isolated network mode tested.
* ✅ Network cleanup completed.

---

# Summary

In this lab, you:

* Learned Docker networking drivers.
* Created custom bridge networks.
* Connected containers using Docker networks.
* Tested container name-based communication.
* Explored host and none networking modes.
* Learned network segmentation and production networking practices.

You are now ready to continue with the next lab in the **Docker** learning path.
