# Lab 17 — Working with Containers

> [!IMPORTANT]
> This lab introduces the Docker container lifecycle and demonstrates how to manage containers using essential Docker commands such as **run, exec, stop, remove, inspect, logs, and resource management**.

## Objective

Master the container lifecycle:

* ▶️ Run containers.
* 🔧 Execute commands inside containers.
* ⏹️ Stop containers.
* 🗑️ Remove containers.
* 🔍 Inspect containers.
* 📋 View container logs.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 16 — Install & Configure Docker on Ubuntu Server**.
* ✅ Docker Engine installed and running.
* ✅ SSH access to your Ubuntu server.
* ✅ Basic Docker command knowledge.

---

# Procedure

## Step 1 — Run a Detached Nginx Container

Start an Nginx container in detached mode:

```bash id="8m4q7v"
docker run -d --name web-server -p 8080:80 nginx
```

Explanation:

| Option   | Description                               |
| -------- | ----------------------------------------- |
| `-d`     | Run container in detached/background mode |
| `--name` | Assign a custom container name            |
| `-p`     | Map host port to container port           |
| `nginx`  | Docker image to run                       |

---

## Step 2 — List Running Containers

View active containers:

```bash id="5n8p3x"
docker ps
```

Example output:

```text id="2v7m9q"
CONTAINER ID   IMAGE   NAME          PORTS
xxxxxx         nginx   web-server    8080->80
```

---

## Step 3 — Execute Commands Inside a Container

Open an interactive shell inside the container:

```bash id="9q4m2k"
docker exec -it web-server bash
```

Inside the container, view the Nginx configuration:

```bash id="6x3m8p"
cat /etc/nginx/nginx.conf
```

Exit the container:

```bash id="1m7v5q"
exit
```

---

## Step 4 — View Container Logs

Display container logs:

```bash id="4p8n6m"
docker logs web-server
```

Follow logs continuously:

```bash id="7k2q9x"
docker logs -f web-server
```

The `-f` option enables live log streaming.

---

## Step 5 — Inspect Container Details

View detailed container information:

```bash id="3v9m5q"
docker inspect web-server
```

Filter container IP address:

```bash id="8q6n4p"
docker inspect web-server | grep -i ipaddress
```

The inspect command provides details including:

* Container configuration.
* Network settings.
* Environment variables.
* Mount information.

---

## Step 6 — Monitor Container Resource Usage

View container statistics:

```bash id="5m8r2k"
docker stats web-server --no-stream
```

Displays:

* CPU usage.
* Memory usage.
* Network traffic.
* Block I/O.

---

## Step 7 — Stop and Remove Container

Stop the running container:

```bash id="2n7q4m"
docker stop web-server
```

Remove the container:

```bash id="9x3p6v"
docker rm web-server
```

---

## Step 8 — Run Container with Resource Limits

Create a container with CPU and memory restrictions:

```bash id="6m4q8v"
docker run -d --name limited-web \
--memory=256m \
--cpus=0.5 \
-p 8081:80 nginx
```

Resource configuration:

| Option     | Value     | Purpose                          |
| ---------- | --------- | -------------------------------- |
| `--memory` | `256m`    | Limits container memory usage    |
| `--cpus`   | `0.5`     | Limits CPU allocation            |
| `-p`       | `8081:80` | Maps host port to container port |

---

# Container Lifecycle Workflow

The Docker container lifecycle follows this process:

```text id="7p3m8q"
Create Container
       |
       ↓
Start Container
       |
       ↓
Execute Commands
       |
       ↓
View Logs / Monitor
       |
       ↓
Stop Container
       |
       ↓
Remove Container
```

---

# Essential Docker Container Commands

| Command          | Purpose                                |
| ---------------- | -------------------------------------- |
| `docker run`     | Create and start a container           |
| `docker ps`      | List running containers                |
| `docker exec`    | Execute commands inside a container    |
| `docker logs`    | View container logs                    |
| `docker inspect` | Display detailed container information |
| `docker stats`   | Monitor resource usage                 |
| `docker stop`    | Stop a running container               |
| `docker rm`      | Remove a container                     |

---

# Best Practice Tips

> [!TIP]
> Apply these practices when managing containers in development and production environments.

## 🏷️ Always Name Containers

Use:

```bash id="0q9v5m"
--name
```

Example:

```bash id="4m7n2x"
docker run --name web-server nginx
```

Benefits:

* Easier administration.
* Clear identification.
* Simplifies troubleshooting.

Avoid anonymous container names because they are difficult to manage.

---

## 🔄 Configure Container Restart Policies

For services that should survive host reboots:

```bash id="5q8m3v"
--restart=unless-stopped
```

Example:

```bash id="8x4n6p"
docker run -d --restart=unless-stopped nginx
```

---

## ⚙️ Limit Container Resources

Always define resource limits:

CPU:

```bash id="1v7m9q"
--cpus
```

Memory:

```bash id="6p3k8x"
--memory
```

Benefits:

* Prevents resource exhaustion.
* Improves server stability.
* Protects other workloads.

---

## 📡 Monitor Container Events

Use:

```bash id="3n5q7m"
docker events
```

Purpose:

* Monitor real-time Docker events.
* Troubleshoot container behavior.
* Identify unexpected lifecycle changes.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Nginx container started successfully.
* ✅ Running containers listed.
* ✅ Commands executed inside a container.
* ✅ Container logs viewed.
* ✅ Container details inspected.
* ✅ Resource usage monitored.
* ✅ Container stopped and removed.
* ✅ Resource-limited container created.

---

# Summary

In this lab, you:

* Managed the Docker container lifecycle.
* Created and ran containers.
* Executed commands inside containers.
* Viewed logs and inspected container details.
* Monitored resource usage.
* Applied CPU and memory restrictions.
* Learned container management best practices.

You are now ready to continue with the next lab in the **Docker** learning path.
