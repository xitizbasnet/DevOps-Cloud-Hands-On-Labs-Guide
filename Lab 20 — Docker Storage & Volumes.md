# Lab 20 — Docker Storage & Volumes

> [!IMPORTANT]
> This lab explains Docker storage mechanisms and demonstrates how to use **named volumes**, **bind mounts**, and **tmpfs storage** for managing persistent and temporary application data.

## Objective

Understand the difference between:

* 💾 Named volumes.
* 📂 Bind mounts.
* ⚡ tmpfs storage.

Learn how to use each storage type for persistent data management.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 17 — Working with Containers**.
* ✅ Docker Engine installed and running.
* ✅ Basic understanding of Docker containers.
* ✅ SSH access to your Ubuntu server.

---

# Docker Storage Overview

Docker provides multiple storage options:

| Storage Type | Description                        | Data Persistence |
| ------------ | ---------------------------------- | ---------------- |
| Named Volume | Managed by Docker                  | Persistent       |
| Bind Mount   | Maps host directory into container | Persistent       |
| tmpfs        | Stored in host memory              | Not persistent   |

---

# Procedure

# Part 1 — Named Volumes

## Step 1 — Create a Docker Named Volume

Create a volume:

```bash id="5n8q2v"
docker volume create mydata
```

List Docker volumes:

```bash id="7m4p9x"
docker volume ls
```

Inspect volume details:

```bash id="3q8m5v"
docker volume inspect mydata
```

Docker manages the volume location automatically.

---

## Step 2 — Run Container with Named Volume

Start a MySQL container using the volume:

```bash id="9v2m6q"
docker run -d --name db \
-v mydata:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root123 \
mysql:8.0
```

Explanation:

| Option                     | Purpose                       |
| -------------------------- | ----------------------------- |
| `-v mydata:/var/lib/mysql` | Mount named volume            |
| `MYSQL_ROOT_PASSWORD`      | Configure MySQL root password |
| `mysql:8.0`                | MySQL Docker image            |

The database files remain available even if the container is removed.

---

# Part 2 — Bind Mounts

## Step 3 — Create Host Directory

Create a local directory:

```bash id="2x7m4n"
mkdir ~/html
```

Create an HTML file:

```bash id="6p9q3m"
echo "<h1>DevOps Lab</h1>" > ~/html/index.html
```

---

## Step 4 — Run Container with Bind Mount

Start an Nginx container:

```bash id="8m5v2q"
docker run -d --name web \
-v ~/html:/usr/share/nginx/html:ro \
-p 8080:80 nginx
```

Explanation:

| Option                  | Purpose             |
| ----------------------- | ------------------- |
| `~/html`                | Host directory      |
| `/usr/share/nginx/html` | Container directory |
| `:ro`                   | Read-only mount     |
| `-p 8080:80`            | Port mapping        |

Access:

```text id="4q8m6x"
http://<SERVER-IP>:8080
```

---

# Part 3 — tmpfs Storage

## Step 5 — Create In-Memory Temporary Storage

Run a container using tmpfs:

```bash id="7x3m9p"
docker run -d --name tmpweb \
--tmpfs /tmp:size=100m \
nginx
```

Characteristics:

* Stored in memory.
* Faster access.
* Data disappears when the container stops.

---

# Part 4 — Backup Docker Volume Data

## Step 6 — Backup Named Volume

Create a compressed backup:

```bash id="5m8q4v"
docker run --rm \
-v mydata:/source \
-v $(pwd):/backup \
ubuntu tar czf /backup/mydata-backup.tar.gz -C /source .
```

This creates:

```text id="9q2m5x"
mydata-backup.tar.gz
```

containing the volume data.

---

# Part 5 — Remove Volume and Container

## Step 7 — Stop and Remove Database Container

Stop container:

```bash id="1v6m8q"
docker stop db
```

Remove container:

```bash id="3n7p5x"
docker rm db
```

Remove volume:

```bash id="8q4m2v"
docker volume rm mydata
```

---

# Docker Storage Workflow

The Docker storage workflow:

```text id="6m9q3v"
Container
    |
    ↓
Storage Requirement
    |
    ↓
Choose Storage Type
    |
    ├── Named Volume
    |
    ├── Bind Mount
    |
    └── tmpfs
    |
    ↓
Application Data Management
```

---

# Storage Type Comparison

| Feature          | Named Volume | Bind Mount            | tmpfs          |
| ---------------- | ------------ | --------------------- | -------------- |
| Managed By       | Docker       | User/Host OS          | Host Memory    |
| Persistent       | ✅ Yes        | ✅ Yes                 | ❌ No           |
| Production Usage | Recommended  | Limited               | Temporary Data |
| Backup Support   | Easy         | Manual                | Not Applicable |
| Performance      | High         | Depends on filesystem | Very Fast      |

---

# Best Practice Tips

> [!TIP]
> Use the correct storage type based on application requirements and security needs.

## 💾 Prefer Named Volumes for Production

Use:

```bash id="4p8n6m"
docker volume
```

instead of bind mounts for production workloads.

Benefits:

* Docker manages storage location.
* Better permissions handling.
* Easier backup and migration.

---

## 🔒 Use Read-Only Mounts for Configuration Files

Use:

```bash id="7m3q9x"
:ro
```

Example:

```bash id="0n6v4p"
-v ./config:/app/config:ro
```

Benefits:

* Prevents container modification.
* Protects configuration files.
* Improves security.

---

## 🗄️ Never Store Database Data in Container Layers

Avoid:

```text id="8x5m2q"
Database data inside container filesystem
```

Always use:

```text id="2q7m9v"
Docker volumes
```

for:

* MySQL.
* PostgreSQL.
* MongoDB.
* Other stateful workloads.

---

## 🔗 Use Docker Compose Volumes

For multi-container applications:

Use the:

```yaml id="9m4q7x"
volumes:
```

section in `docker-compose.yml`.

Benefits:

* Share storage between services.
* Simplify deployments.
* Manage application environments easily.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Named volume created.
* ✅ Container launched with named volume.
* ✅ Bind mount configured.
* ✅ Read-only mount tested.
* ✅ tmpfs storage created.
* ✅ Volume backup completed.
* ✅ Docker volume removed successfully.

---

# Summary

In this lab, you:

* Learned Docker storage options.
* Created and managed named volumes.
* Used bind mounts for host-container file sharing.
* Tested temporary tmpfs storage.
* Backed up Docker volume data.
* Applied Docker storage best practices.

You are now ready to continue with the next lab in the **Docker** learning path.
