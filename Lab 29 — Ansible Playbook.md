# Lab 29 — Ansible Playbook

> [!IMPORTANT]
> This lab demonstrates how to create a complete **Ansible Playbook** to install and configure an Nginx web server using automation, variables, templates, handlers, and controlled execution options.

## Objective

Write and execute a complete Ansible Playbook to:

* 🌐 Install Nginx web server.
* 📁 Create a document root directory.
* 📄 Deploy an HTML page using templates.
* ⚙️ Configure and manage Nginx service.
* 🔄 Reload services using Ansible handlers.

---

## Prerequisites

Before starting, ensure:

* ✅ Ansible Controller is configured.
* ✅ Target nodes are reachable through SSH.
* ✅ Inventory file is available.
* ✅ User has privilege escalation access (`sudo`).

Example structure:

```text
ansible-lab/
│
├── inventory.ini
├── nginx-setup.yml
└── index.html.j2
```

---

# Ansible Playbook Overview

Ansible Playbook workflow:

```text
Create YAML Playbook
        |
        ↓
Define Hosts & Variables
        |
        ↓
Execute Tasks
        |
        ↓
Trigger Handlers
        |
        ↓
Validate Deployment
```

---

# Procedure

# Part 1 — Create Nginx Playbook

## Step 1 — Create Playbook File

Create:

```bash
nano nginx-setup.yml
```

Add the following configuration:

```yaml
---
- name: Install and Configure Nginx Web Server

  hosts: webservers

  become: true

  gather_facts: true

  vars:
    nginx_port: 80
    server_name: "devops-app.example.com"
    doc_root: /var/www/html


  tasks:

    - name: Update apt cache
      apt:
        update_cache: yes
        cache_valid_time: 3600


    - name: Install Nginx
      apt:
        name: nginx
        state: latest


    - name: Create document root
      file:
        path: "{{ doc_root }}"
        state: directory
        mode: '0755'
        owner: www-data


    - name: Deploy index.html
      template:
        src: index.html.j2
        dest: "{{ doc_root }}/index.html"
        mode: '0644'
      notify:
        - Reload Nginx


    - name: Ensure Nginx is running
      service:
        name: nginx
        state: started
        enabled: yes


  handlers:

    - name: Reload Nginx
      service:
        name: nginx
        state: reloaded
```

---

# Playbook Configuration Explanation

## Playbook Header

```yaml
hosts: webservers
```

Defines the target inventory group.

---

```yaml
become: true
```

Enables privilege escalation using sudo.

---

```yaml
gather_facts: true
```

Collects system information before executing tasks.

---

# Variables Section

```yaml
vars:
  nginx_port: 80
  server_name: "devops-app.example.com"
  doc_root: /var/www/html
```

Variables allow:

* Easier customization.
* Reusable configurations.
* Environment-specific changes.

---

# Tasks Explanation

## Update Package Cache

```yaml
apt:
  update_cache: yes
```

Refreshes Ubuntu package metadata.

---

## Install Nginx

```yaml
apt:
  name: nginx
  state: latest
```

Ensures the latest Nginx package is installed.

---

## Create Document Root

```yaml
file:
  path: "{{ doc_root }}"
```

Creates:

```text
/var/www/html
```

with:

* Correct permissions.
* Correct ownership.

---

## Deploy Website File

```yaml
template:
  src: index.html.j2
```

Copies a Jinja2 template to:

```text
/var/www/html/index.html
```

---

## Start Nginx Service

```yaml
service:
  name: nginx
  state: started
```

Ensures:

* Nginx is running.
* Nginx starts automatically after reboot.

---

# Handlers

Handlers execute only when notified.

Example:

```yaml
notify:
  - Reload Nginx
```

Handler:

```yaml
handlers:
  - name: Reload Nginx
```

Benefits:

* Avoids unnecessary service restarts.
* Executes once after all tasks complete.

---

# Part 2 — Execute Playbook

## Step 2 — Run Playbook

Execute:

```bash
ansible-playbook -i inventory.ini nginx-setup.yml
```

Expected result:

```text
PLAY RECAP

web01    ok=5 changed=3 failed=0
web02    ok=5 changed=3 failed=0
```

---

# Part 3 — Run Playbook with Options

## Run with Verbose Output

Command:

```bash
ansible-playbook -i inventory.ini nginx-setup.yml -v
```

Provides additional execution details.

---

## Perform Dry Run

Command:

```bash
ansible-playbook -i inventory.ini nginx-setup.yml --check
```

Purpose:

* Shows planned changes.
* Does not modify systems.

---

## Run Specific Tags

Command:

```bash
ansible-playbook -i inventory.ini nginx-setup.yml --tags install
```

Runs only tasks associated with:

```text
install
```

tags.

---

## Limit Execution to Specific Host

Command:

```bash
ansible-playbook -i inventory.ini nginx-setup.yml --limit web01
```

Runs playbook only on:

```text
web01
```

---

# Playbook Execution Flow

```text
Start Playbook
      |
      ↓
Gather Facts
      |
      ↓
Update Package Cache
      |
      ↓
Install Nginx
      |
      ↓
Create Web Directory
      |
      ↓
Deploy Website
      |
      ↓
Reload Nginx
      |
      ↓
Verify Service
```

---

# Useful Ansible Playbook Commands

| Command                     | Purpose                   |
| --------------------------- | ------------------------- |
| `ansible-playbook file.yml` | Execute playbook          |
| `--check`                   | Dry run                   |
| `-v`                        | Verbose output            |
| `--tags`                    | Run selected tasks        |
| `--limit`                   | Target specific hosts     |
| `ansible-lint`              | Validate playbook quality |

---

# Best Practice Tips

> [!TIP]
> Follow these recommendations when creating production Ansible Playbooks.

---

## 🔄 Use Handlers for Service Restarts

Handlers:

* Run only when triggered.
* Execute once after tasks complete.
* Avoid unnecessary restarts.

Example:

```yaml
notify:
  - Reload Nginx
```

---

## 📄 Use Jinja2 Templates

Use:

```text
.j2
```

template files for:

* Configuration files.
* Environment-specific values.
* Version-controlled deployments.

Benefits:

* Parameterized configuration.
* Easier maintenance.
* Reusable automation.

---

## 🧱 Organize Tasks Using Blocks

Use:

```yaml
block:
```

for grouping related tasks.

Benefits:

* Cleaner playbooks.
* Better error handling.
* Supports:

```yaml
rescue:
always:
```

sections.

---

## 🔍 Validate Playbooks Before Commit

Run:

```bash
ansible-lint nginx-setup.yml
```

Benefits:

* Detects syntax issues.
* Improves code quality.
* Enforces automation standards.

---

# Validation Checklist

Verify:

* ✅ Playbook syntax is correct.
* ✅ Nginx package installed.
* ✅ Document root created.
* ✅ HTML template deployed.
* ✅ Nginx service running.
* ✅ Handler reload executed.
* ✅ Playbook passes lint validation.

---

# Summary

In this lab, you:

* Created an Ansible Playbook.
* Automated Nginx installation.
* Used variables and templates.
* Implemented handlers.
* Executed controlled deployments.
* Applied Ansible automation best practices.

You are now ready to build multi-role Ansible automation using roles, variables, and reusable playbook structures.
