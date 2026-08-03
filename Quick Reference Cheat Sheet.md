# Quick Reference Cheat Sheet

## Overview

This quick reference guide provides commonly used commands, practices, and operational tips for DevOps and IT engineering tools.

Use this cheat sheet for daily administration, troubleshooting, automation, monitoring, and infrastructure management tasks.

---

# 📘 DevOps Tool Command Reference

| Tool / Technology | Key Commands / Tips                                                                   |
| ----------------- | ------------------------------------------------------------------------------------- |
| 🐳 **Docker**     | `docker ps` | `docker exec -it` | `docker logs -f` | `docker system prune`            |
| ☸️ **Kubernetes** | `kubectl get pods -A` | `kubectl describe` | `kubectl rollout` | `kubectl top`        |
| 🏗️ **Terraform** | `terraform init` → `validate` → `plan` → `apply` → `destroy` | Use remote state in S3 |
| ⚙️ **Ansible**    | `ansible-playbook -i inv site.yml --check` | `ansible-vault` | `ansible-lint`         |
| 🧩 **Jenkins**    | Jenkinsfile stored in SCM | Blue Ocean UI | Shared Libraries for DRY pipelines        |
| 🌿 **Git**        | `git log --oneline --graph` | `git stash` | `git rebase` | `git cherry-pick`          |
| ☁️ **AWS CLI**    | `aws configure` | `aws sts get-caller-identity` | `--output json` | `--query`         |
| 📊 **ELK Stack**  | Filebeat → Elasticsearch → Kibana | ILM for retention | Index templates               |
| 📈 **Prometheus** | PromQL functions: `rate()` | `avg_over_time()` | `histogram_quantile()`               |
| 🖥️ **Bash**      | `set -euo pipefail` | `${VAR:-default}` | `trap` | `shellcheck`                       |

---

# 🐳 Docker Quick Reference

## Common Commands

```bash
docker ps
```

Lists running containers.

```bash
docker exec -it <container>
```

Opens an interactive shell session inside a running container.

```bash
docker logs -f <container>
```

Streams container logs in real time.

```bash
docker system prune
```

Removes unused Docker resources.

> [!WARNING]
> Use `docker system prune` carefully because it can remove unused containers, images, networks, and build cache.

---

# ☸️ Kubernetes Quick Reference

## Common Commands

```bash
kubectl get pods -A
```

Lists pods across all namespaces.

```bash
kubectl describe <resource>
```

Displays detailed information about Kubernetes resources.

```bash
kubectl rollout
```

Manages application deployment rollouts.

```bash
kubectl top
```

Displays CPU and memory usage information.

---

# 🏗️ Terraform Quick Reference

## Standard Workflow

```text
terraform init
        ↓
terraform validate
        ↓
terraform plan
        ↓
terraform apply
        ↓
terraform destroy
```

## Recommended Practice

Use remote state storage:

```text
Terraform State → AWS S3 Remote Backend
```

Benefits:

* Enables team collaboration.
* Provides centralized state management.
* Supports state locking.

---

# ⚙️ Ansible Quick Reference

## Run Playbook Validation

```bash
ansible-playbook -i inv site.yml --check
```

Performs a dry run without applying changes.

## Security and Quality Tools

| Command         | Purpose                            |
| --------------- | ---------------------------------- |
| `ansible-vault` | Encrypts sensitive information     |
| `ansible-lint`  | Validates Ansible playbook quality |

---

# 🧩 Jenkins Quick Reference

## Recommended Practices

### Store Jenkinsfile in Source Control Management (SCM)

Benefits:

* Version-controlled pipelines.
* Improved collaboration.
* Easier rollback.

### Use Blue Ocean UI

Benefits:

* Visual pipeline representation.
* Easier troubleshooting.
* Better pipeline monitoring.

### Use Shared Libraries

Purpose:

* Avoid duplicate pipeline code.
* Maintain reusable CI/CD components.
* Follow DRY (Don't Repeat Yourself) principles.

---

# 🌿 Git Quick Reference

## Useful Commands

```bash
git log --oneline --graph
```

Displays a compact visual commit history.

```bash
git stash
```

Temporarily stores uncommitted changes.

```bash
git rebase
```

Reapplies commits onto another branch.

```bash
git cherry-pick
```

Applies specific commits from another branch.

---

# ☁️ AWS CLI Quick Reference

## Configure AWS CLI

```bash
aws configure
```

Sets AWS credentials and default configuration.

---

## Verify Identity

```bash
aws sts get-caller-identity
```

Displays the currently authenticated AWS account and user information.

---

## Output Formatting

```bash
--output json
```

Returns command output in JSON format.

```bash
--query
```

Filters AWS CLI response data.

Example:

```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].InstanceId"
```

---

# 📊 ELK Stack Quick Reference

## Data Flow

```text
Filebeat
    ↓
Elasticsearch
    ↓
Kibana
```

## Best Practices

| Feature                          | Purpose                                       |
| -------------------------------- | --------------------------------------------- |
| ILM (Index Lifecycle Management) | Manages index retention and rollover policies |
| Index Templates                  | Defines index configuration standards         |

---

# 📈 Prometheus Quick Reference

## Common PromQL Functions

| Function               | Purpose                                             |
| ---------------------- | --------------------------------------------------- |
| `rate()`               | Calculates per-second rate changes over time        |
| `avg_over_time()`      | Calculates average values over a time range         |
| `histogram_quantile()` | Calculates percentile values from histogram metrics |

Example:

```promql
rate(http_requests_total[5m])
```

---

# 🖥️ Bash Quick Reference

## Recommended Shell Safety Settings

```bash
set -euo pipefail
```

Enables safer script execution:

* `-e` → Exit on command failure.
* `-u` → Fail on undefined variables.
* `pipefail` → Detect pipeline failures.

---

## Default Variable Values

```bash
${VAR:-default}
```

Uses a default value when a variable is empty or undefined.

Example:

```bash
PORT=${PORT:-8080}
```

---

## Trap Handling

```bash
trap
```

Executes commands when specific signals occur.

Common uses:

* Cleanup temporary files.
* Handle script termination.
* Perform graceful shutdown.

---

## Shell Script Validation

```bash
shellcheck
```

Analyzes shell scripts for common errors and best-practice improvements.

---

# 📚 Summary

This cheat sheet provides a compact operational reference for:

* Container management.
* Kubernetes administration.
* Infrastructure automation.
* Configuration management.
* CI/CD engineering.
* Cloud operations.
* Monitoring and observability.
* Shell scripting best practices.

Keep this reference available during daily DevOps and IT operations activities.
