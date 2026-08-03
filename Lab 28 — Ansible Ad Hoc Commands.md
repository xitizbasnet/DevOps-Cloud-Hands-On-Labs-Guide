# Lab 28 — Ansible Ad Hoc Commands

> [!IMPORTANT]
> This lab demonstrates how to execute **quick one-off automation tasks** on managed nodes using Ansible ad hoc commands without creating a playbook.

## Objective

Run immediate tasks on Ansible-managed nodes without writing a playbook.

Learn how to:

* 📡 Test node connectivity.
* 🖥️ Execute remote commands.
* 📦 Install packages.
* 📁 Copy files.
* ⚙️ Manage services.
* 🔍 Gather system information.
* 👤 Create users.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 27 — Install & Configure Ansible Controller & Target Node**.
* ✅ Ansible installed on the controller node.
* ✅ Inventory file configured.
* ✅ SSH key-based authentication working.

Example inventory:

```text id="4m8q7v"
ansible-lab/
│
├── inventory.ini
└── ansible.cfg
```

---

# Ansible Ad Hoc Commands Overview

Ansible ad hoc commands are useful for:

* Emergency fixes.
* Quick system checks.
* One-time administrative tasks.
* Testing connectivity.

Workflow:

```text id="8q3n5m"
Controller Node
       |
       ↓
Ansible Command
       |
       ↓
Inventory File
       |
       ↓
Managed Nodes
```

---

# Procedure

# Part 1 — Test Host Connectivity

## Step 1 — Ping All Hosts

Test communication with all managed nodes:

```bash id="6p9m2x"
ansible all -i inventory.ini -m ping
```

Expected response:

```text id="2v7m9q"
SUCCESS => {
    "ping": "pong"
}
```

---

# Part 2 — Execute Remote Shell Commands

## Step 2 — Check Disk Usage on Web Servers

Run:

```bash id="9m4p6q"
ansible webservers -i inventory.ini -m shell -a 'df -h'
```

Purpose:

* Checks filesystem usage.
* Helps identify disk space issues.

---

# Part 3 — Install Packages

## Step 3 — Install Nginx Package

Install Nginx on web servers:

```bash id="5x8n2v"
ansible webservers -i inventory.ini -m apt \
-a 'name=nginx state=present' --become
```

Explanation:

| Option          | Purpose                  |
| --------------- | ------------------------ |
| `-m apt`        | Uses Ansible apt module  |
| `name=nginx`    | Package name             |
| `state=present` | Ensures package exists   |
| `--become`      | Run with sudo privileges |

---

# Part 4 — Copy Files to Managed Nodes

## Step 4 — Transfer File

Copy `index.html`:

```bash id="7q3m8p"
ansible webservers -i inventory.ini -m copy \
-a 'src=./index.html dest=/tmp/index.html mode=0644'
```

Configuration:

| Parameter | Purpose             |
| --------- | ------------------- |
| `src`     | Local file location |
| `dest`    | Remote destination  |
| `mode`    | File permission     |

---

# Part 5 — Manage Services

## Step 5 — Start and Enable Nginx

Manage Nginx service:

```bash id="1m8p5x"
ansible webservers -i inventory.ini -m service \
-a 'name=nginx state=started enabled=yes' --become
```

This ensures:

* Nginx is running.
* Nginx starts automatically after reboot.

---

# Part 6 — Gather System Facts

## Step 6 — Collect OS Information

Gather system facts:

```bash id="8v5m2x"
ansible web01 -i inventory.ini -m setup | grep ansible_os
```

Returns information about:

* Operating system.
* Distribution.
* System details.

---

# Part 7 — Create Users

## Step 7 — Create DevOps User

Create user:

```bash id="4n7m9x"
ansible webservers -i inventory.ini -m user \
-a 'name=devops state=present shell=/bin/bash' --become
```

Creates:

```text id="6m3q8p"
User: devops
Shell: /bin/bash
```

---

# Part 8 — Check Disk Space

## Step 8 — Check Root Filesystem Usage

Run:

```bash id="9p5m2v"
ansible all -i inventory.ini -m shell -a 'df -h /' \
| grep -v WARNING
```

Purpose:

* Checks root filesystem usage.
* Removes warning messages from output.

---

# Common Ansible Ad Hoc Modules

| Module    | Purpose           | Example Usage |
| --------- | ----------------- | ------------- |
| `ping`    | Test connectivity | `-m ping`     |
| `shell`   | Execute commands  | `-m shell`    |
| `apt`     | Manage packages   | `-m apt`      |
| `copy`    | Copy files        | `-m copy`     |
| `service` | Manage services   | `-m service`  |
| `setup`   | Gather facts      | `-m setup`    |
| `user`    | Manage users      | `-m user`     |

---

# Useful Command Pattern

General syntax:

```bash id="3q8m6v"
ansible <group_or_host> \
-i <inventory_file> \
-m <module> \
-a '<arguments>'
```

Example:

```bash id="5m2q8v"
ansible webservers -i inventory.ini -m ping
```

---

# Best Practice Tips

> [!TIP]
> Use ad hoc commands for quick operations, but use playbooks for repeatable automation.

---

## 🧪 Use Check Mode Before Applying Changes

Perform a dry run:

```bash id="7v2m9q"
ansible ... --check
```

Benefits:

* Shows expected changes.
* Prevents accidental modifications.
* Improves confidence before execution.

---

## 🐞 Increase Verbosity for Debugging

Use:

```bash id="2q8m5v"
-v
```

More detailed output:

```bash id="6x4m8p"
-vv
```

Maximum debugging information:

```bash id="9m3q7p"
-vvv
```

---

## 📘 Use Playbooks for Repeatable Automation

Ad hoc commands are best for:

* Quick fixes.
* Testing.
* Simple checks.

For production automation use:

```text id="8p5m2v"
Ansible Playbooks
```

Benefits:

* Version controlled.
* Repeatable.
* Auditable.
* Team friendly.

---

## 📚 Check Module Documentation Locally

List available modules:

```bash id="4q8m6v"
ansible-doc -l
```

Search for a module:

```bash id="1v7m9q"
ansible-doc -l | grep module_name
```

View module details:

```bash id="3m6q8p"
ansible-doc module_name
```

---

# Validation Checklist

Verify:

* ✅ All hosts respond to ping.
* ✅ Remote shell commands executed.
* ✅ Packages installed successfully.
* ✅ Files copied to target nodes.
* ✅ Services started and enabled.
* ✅ System facts collected.
* ✅ Users created.
* ✅ Disk usage checked.

---

# Summary

In this lab, you:

* Executed Ansible ad hoc commands.
* Used common Ansible modules.
* Managed packages and services.
* Copied files remotely.
* Gathered system information.
* Learned when to use ad hoc commands versus playbooks.

You are now ready to automate repeatable infrastructure tasks using Ansible Playbooks.
