# Lab 30 — Using Variables and Facts in Ansible

> [!IMPORTANT]
> This lab demonstrates how to use **Ansible variables and facts** to create flexible, reusable automation. You will work with `group_vars`, `host_vars`, registered variables, custom facts, and system information collected through Ansible facts.

## Objective

Learn how to use:

* 📁 `group_vars` for group-level variables.
* 🖥️ `host_vars` for host-specific variables.
* 🔍 `ansible_facts` for system information.
* 📝 `register` for storing command output.
* ⚙️ `set_fact` for creating runtime variables.
* 🔀 Conditional execution using facts.

---

# Prerequisites

Before starting, ensure:

* ✅ Ansible Controller is configured.
* ✅ Inventory file exists.
* ✅ Target hosts are reachable.
* ✅ Previous Ansible labs are completed.

---

# Ansible Variable Structure

Recommended project layout:

```text
ansible-lab/
│
├── inventory.ini
│
├── group_vars/
│   ├── all.yml
│   └── webservers.yml
│
├── host_vars/
│   └── web01.yml
│
└── playbooks/
    └── vars-demo.yml
```

---

# Variable Locations Overview

| Location                    | Scope            | Example                |
| --------------------------- | ---------------- | ---------------------- |
| `group_vars/all.yml`        | All hosts        | Company settings       |
| `group_vars/webservers.yml` | Web server group | Web configuration      |
| `host_vars/web01.yml`       | Single host      | Host-specific settings |
| Playbook variables          | Play execution   | Temporary values       |

---

# Part 1 — Configure Group Variables

## Step 1 — Create Global Variables

File:

```text
group_vars/all.yml
```

Content:

```yaml
---
company: "DevOps Corp"

env: "development"

ntp_server: "0.in.pool.ntp.org"
```

These variables apply to:

* All managed hosts.
* All playbooks using this inventory.

---

## Step 2 — Create Web Server Variables

File:

```text
group_vars/webservers.yml
```

Content:

```yaml
---
http_port: 80

max_connections: 1000
```

These variables apply only to:

```text
webservers
```

group.

---

# Part 2 — Configure Host Variables

## Step 3 — Create Host-Specific Variables

File:

```text
host_vars/web01.yml
```

Content:

```yaml
---
```

Host variables apply only to:

```text
web01
```

Example usage:

* Custom ports.
* Environment overrides.
* Host-specific configuration.

---

# Part 3 — Create Variables and Facts Playbook

## Step 4 — Create Playbook

Create:

```bash
nano playbooks/vars-demo.yml
```

Add:

```yaml
---
- name: Variables and Facts Demo

  hosts: webservers

  become: true


  tasks:

    - name: Print OS facts
      debug:
        msg: "OS: {{ ansible_distribution }} {{ ansible_distribution_version }}"


    - name: Get memory info
      debug:
        msg: "Total RAM: {{ ansible_memtotal_mb }} MB"


    - name: Capture command output with register
      shell: uptime
      register: uptime_result


    - name: Print registered variable
      debug:
        var: uptime_result.stdout


    - name: Set a custom fact
      set_fact:
        app_version: "2.1.0"
        deploy_time: "{{ ansible_date_time.iso8601 }}"


    - name: Use custom fact
      debug:
        msg: "Deploying version {{ app_version }} at {{ deploy_time }}"


    - name: Conditional based on fact
      apt:
        name: python3
        state: present

      when: ansible_distribution == "Ubuntu"
```

---

# Playbook Task Explanation

## 🔍 Display Operating System Facts

Task:

```yaml
debug:
  msg: "OS: {{ ansible_distribution }}"
```

Uses:

```text
ansible_distribution
ansible_distribution_version
```

Example output:

```text
OS: Ubuntu 22.04
```

---

## 💾 Display Memory Information

Task:

```yaml
msg: "Total RAM: {{ ansible_memtotal_mb }} MB"
```

Uses:

```text
ansible_memtotal_mb
```

Example:

```text
Total RAM: 4096 MB
```

---

## 📝 Capture Command Output Using Register

Task:

```yaml
shell: uptime
register: uptime_result
```

Stores command output in:

```text
uptime_result
```

Example usage:

```yaml
debug:
  var: uptime_result.stdout
```

---

## ⚙️ Create Custom Facts Using set_fact

Task:

```yaml
set_fact:
  app_version: "2.1.0"
```

Creates runtime variables:

```text
app_version
deploy_time
```

These variables are available during the current play execution.

---

## 🔀 Conditional Task Execution

Example:

```yaml
when: ansible_distribution == "Ubuntu"
```

The task runs only when:

```text
Operating System = Ubuntu
```

Benefits:

* Supports multiple operating systems.
* Prevents incompatible tasks.
* Improves portability.

---

# Execute Playbook

## Step 5 — Run Variables Demo

Command:

```bash
ansible-playbook -i inventory.ini playbooks/vars-demo.yml
```

---

# Example Execution Flow

```text
Gather Facts
      |
      ↓
Read group_vars
      |
      ↓
Read host_vars
      |
      ↓
Execute Tasks
      |
      ↓
Register Output
      |
      ↓
Create Custom Facts
      |
      ↓
Run Conditional Tasks
```

---

# Understanding Ansible Facts

Ansible facts are automatically collected information about managed nodes.

Examples:

| Fact                           | Description           |
| ------------------------------ | --------------------- |
| `ansible_distribution`         | Operating system name |
| `ansible_distribution_version` | OS version            |
| `ansible_memtotal_mb`          | Total memory          |
| `ansible_date_time`            | System date/time      |
| `ansible_hostname`             | Hostname              |

View all facts:

```bash
ansible all -m setup
```

---

# Variable Precedence Order

When multiple variables have the same name, Ansible follows precedence rules:

```text
Highest Priority
        |
        ↓
extra-vars

task vars

playbook vars

host_vars

group_vars

Lowest Priority
```

---

# Best Practice Tips

> [!TIP]
> Follow these practices to create maintainable and secure Ansible automation.

---

## 🔐 Use Ansible Vault for Sensitive Data

Encrypt sensitive variables:

```bash
ansible-vault encrypt group_vars/all.yml
```

Use Vault for:

* Passwords.
* API keys.
* Tokens.
* Secrets.

---

## 🌐 Avoid Hardcoding IP Addresses

Avoid:

```yaml
server_ip: 192.168.1.10
```

Prefer:

```text
inventory hostnames
```

and resolve using:

* DNS.
* Dynamic inventory.
* Cloud discovery.

---

## 🖥️ Use ansible_facts for OS-Specific Tasks

Recommended:

```yaml
when: ansible_facts['distribution'] == "Ubuntu"
```

Benefits:

* Cross-platform playbooks.
* Cleaner automation.
* Better compatibility.

---

# Validation Checklist

Verify:

* ✅ Directory structure created.
* ✅ Group variables configured.
* ✅ Host variables configured.
* ✅ Facts displayed successfully.
* ✅ Command output registered.
* ✅ Custom facts created.
* ✅ Conditional tasks executed correctly.
* ✅ Sensitive variables protected.

---

# Summary

In this lab, you:

* Created structured Ansible variables.
* Used `group_vars` and `host_vars`.
* Retrieved system information using facts.
* Stored command results using `register`.
* Created runtime values with `set_fact`.
* Used conditions for operating system-specific automation.

You are now ready to build more dynamic and environment-aware Ansible automation.
