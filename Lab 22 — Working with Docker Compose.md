# Lab 22 — Working with Docker Compose

> [!IMPORTANT]
> This lab demonstrates how to define and run multi-container applications using a `docker-compose.yml` file. You will configure multiple services, manage application dependencies, and control the complete application stack lifecycle.

## Objective

Define and run multi-container applications using:

```yaml
docker-compose.yml
```

Learn how to:

* 🧩 Define multiple services.
* 🔗 Configure service dependencies.
* 💾 Manage persistent storage.
* ▶️ Start and stop application stacks.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 21 — Docker Networking**.
* ✅ Docker Engine installed.
* ✅ Docker Compose plugin installed.
* ✅ Basic understanding of Docker containers and images.

---

# Docker Compose Overview

Docker Compose allows you to define an entire application stack in a single YAML configuration file.

A Compose application can include:

* Application containers.
* Databases.
* Networks.
* Persistent storage.
* Environment variables.

Example architecture:

```text
                    Docker Compose Stack

        ┌──────────────┐
        │     Web      │
        │    Nginx     │
        └──────┬───────┘
               |
               ↓
        ┌──────────────┐
        │     App      │
        │   Backend    │
        └──────┬───────┘
               |
               ↓
        ┌──────────────┐
        │      DB      │
        │    MySQL     │
        └──────────────┘
```

---

# Procedure

# Part 1 — Verify Docker Compose

## Step 1 — Check Docker Compose Version

Verify Docker Compose installation:

```bash
docker compose version
```

Example output:

```text
Docker Compose version v2.x.x
```

---

# Part 2 — Create Compose Project

## Step 2 — Create Project Directory

Create a workspace:

```bash
mkdir ~/compose-lab
cd ~/compose-lab
```

---

## Step 3 — Create docker-compose.yml

Create the Compose configuration file:

```bash
nano docker-compose.yml
```

Add the following content:

```yaml
version: '3.8'

services:

  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html:ro
    depends_on:
      - app
    restart: unless-stopped

  app:
    build: ./app
    ports:
      - "5000:5000"
    environment:
      - DB_HOST=db
      - DB_PORT=3306
    depends_on:
      - db
    restart: unless-stopped

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: devopsdb
    volumes:
      - db_data:/var/lib/mysql
    restart: unless-stopped


volumes:
  db_data:


networks:
  default:
    name: devops-stack
```

---

# Understanding docker-compose.yml

## Services

The Compose file defines three services:

| Service | Image        | Purpose             |
| ------- | ------------ | ------------------- |
| `web`   | nginx:alpine | Web frontend        |
| `app`   | Custom build | Application backend |
| `db`    | mysql:8.0    | Database service    |

---

## Volumes

Persistent database storage:

```yaml
volumes:
  db_data:
```

Mounted into MySQL:

```yaml
- db_data:/var/lib/mysql
```

---

## Networks

Custom Compose network:

```yaml
networks:
  default:
    name: devops-stack
```

All services communicate through:

```text
devops-stack
```

---

# Part 3 — Manage Compose Application

## Step 4 — Start All Services

Run the application stack in detached mode:

```bash
docker compose up -d
```

The `-d` option runs services in the background.

---

## Step 5 — Check Service Status

View running services:

```bash
docker compose ps
```

Displays:

* Service names.
* Container status.
* Port mappings.

---

## Step 6 — View Application Logs

Follow application logs:

```bash
docker compose logs -f app
```

The `-f` option enables live log streaming.

---

## Step 7 — Access Database Container

Connect to MySQL:

```bash
docker compose exec db mysql -u root -p
```

Enter the configured password:

```text
rootpass
```

---

## Step 8 — Stop Compose Application

Stop and remove containers:

```bash
docker compose down
```

This removes:

* Containers.
* Networks.

---

## Step 9 — Remove Application and Volumes

Remove containers and persistent volumes:

```bash
docker compose down -v
```

This also removes:

* Named volumes.
* Stored database data.

---

# Docker Compose Lifecycle

```text
Create docker-compose.yml
          |
          ↓
docker compose up
          |
          ↓
Services Running
          |
          ↓
Monitor Logs
          |
          ↓
Update Configuration
          |
          ↓
docker compose down
```

---

# Common Docker Compose Commands

| Command                  | Purpose                          |
| ------------------------ | -------------------------------- |
| `docker compose up -d`   | Start services in background     |
| `docker compose ps`      | View service status              |
| `docker compose logs -f` | Follow service logs              |
| `docker compose exec`    | Execute commands inside services |
| `docker compose down`    | Stop and remove stack            |
| `docker compose down -v` | Remove stack and volumes         |

---

# Best Practice Tips

> [!TIP]
> Follow these practices when managing multi-container applications.

## 🔐 Use `.env` Files for Configuration

Instead of storing secrets directly:

Create:

```text
.env
```

Example:

```env
MYSQL_ROOT_PASSWORD=secure_password
DB_HOST=db
```

Docker Compose automatically reads `.env` files.

Benefits:

* Protects sensitive data.
* Simplifies environment changes.
* Supports different deployment configurations.

---

## 🩺 Use Health Checks for Service Startup

Use:

```yaml
healthcheck:
```

with:

```yaml
depends_on:
```

Benefits:

* Controls startup order.
* Ensures dependencies are ready.
* Prevents application startup failures.

---

## 🚀 Use Orchestration Platforms for Production

For production environments, prefer:

* Docker Swarm.
* Kubernetes.

Docker Compose is best suited for:

* Development.
* Testing.
* Small deployments.

---

## 📂 Version Control Compose Files

Store:

```text
docker-compose.yml
```

alongside application source code.

Benefits:

* Infrastructure as code.
* Easy environment recreation.
* Team collaboration.
* Deployment consistency.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Docker Compose version verified.
* ✅ Compose project created.
* ✅ Multi-container YAML configuration created.
* ✅ Web, application, and database services defined.
* ✅ Services started successfully.
* ✅ Service status checked.
* ✅ Application logs viewed.
* ✅ Database accessed.
* ✅ Compose stack stopped successfully.

---

# Summary

In this lab, you:

* Learned Docker Compose fundamentals.
* Created a multi-container application stack.
* Defined services using YAML.
* Configured networking and persistent volumes.
* Managed application lifecycle commands.
* Applied Docker Compose best practices.

You are now ready to continue with the next lab in the **Docker** learning path.
