# Section 6 — Ansible

# Lab 27 — Install & Configure Ansible Controller & Target Node

> [!IMPORTANT]
> This lab demonstrates how to set up an **Ansible Controller node**, configure **SSH-based passwordless authentication**, create an inventory file, and verify communication with managed target nodes.

## Objective

Set up:

* 🧠 Ansible Controller node.
* 🖥️ Managed Target nodes.
* 🔑 SSH key-based passwordless authentication.

Configure Ansible to manage remote Linux systems.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Ubuntu-based controller node.
* ✅ One or more Ubuntu target nodes.
* ✅ SSH access to all nodes.
* ✅ Private IP addresses or reachable network connectivity.
* ✅ Python installed on target nodes.

Example architecture:

```text id="4m8q7v"
              Ansible Controller
                    |
                    |
          SSH Key Authentication
                    |
     ┌──────────────┼──────────────┐
     ↓              ↓              ↓
  web01          web02          db01
 Target         Target        Target
 Node           Node          Node
```

---

# Procedure

# Part 1 — Install Ansible on Controller Node

> Perform these steps on the Ansible Controller node.

---

## Step 1 — Update Package Repository

Run:

```bash id="8q3n5m"
sudo apt update
```

---

## Step 2 — Install Required Repository Tools

Install software properties:

```bash id="6p9m2x"
sudo apt install -y software-properties-common
```

---

## Step 3 — Add Ansible Repository

Add the official Ansible PPA:

```bash id="2v7m9q"
sudo add-apt-repository --yes --update ppa:ansible/ansible
```

---

## Step 4 — Install Ansible

Install Ansible:

```bash id="9m4p6q"
sudo apt install -y ansible
```

Verify installation:

```bash id="5x8n2v"
ansible --version
```

Example output:

```text id="7q3m8p"
ansible [core x.x.x]
```

---

# Part 2 — Configure Passwordless SSH Authentication

Ansible uses SSH to communicate with managed nodes.

---

## Step 5 — Generate SSH Key Pair

On the controller node:

```bash id="1m8p5x"
ssh-keygen -t ed25519 -f ~/.ssh/ansible_key -N ''
```

Creates:

```text id="8v5m2x"
~/.ssh/ansible_key
~/.ssh/ansible_key.pub
```

---

## Step 6 — Copy Public Key to Target Nodes

Copy the SSH public key:

### Target Node 1

```bash id="4n7m9x"
ssh-copy-id -i ~/.ssh/ansible_key.pub ubuntu@<TARGET-1-IP>
```

### Target Node 2

```bash id="6m3q8p"
ssh-copy-id -i ~/.ssh/ansible_key.pub ubuntu@<TARGET-2-IP>
```

Replace:

```text id="3q8m6v"
<TARGET-1-IP>
<TARGET-2-IP>
```

with actual target node IP addresses.

---

## Step 7 — Test SSH Connection

Verify passwordless SSH:

```bash id="9p5m2v"
ssh -i ~/.ssh/ansible_key ubuntu@<TARGET-1-IP> 'hostname'
```

Expected:

```text id="5m8q2x"
web01
```

or the configured hostname.

---

# Part 3 — Create Ansible Inventory

The inventory file defines managed hosts and connection details.

---

## Step 8 — Create Ansible Project Directory

Create workspace:

```bash id="7v2m9q"
mkdir ~/ansible-lab
cd ~/ansible-lab
```

---

## Step 9 — Create Inventory File

Create:

```bash id="2q8m5v"
nano inventory.ini
```

Add:

```ini id="6x4m8p"
[webservers]

web01
ansible_host=<TARGET-1-IP>
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/ansible_key

web02
ansible_host=<TARGET-2-IP>
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/ansible_key


[dbservers]

db01
ansible_host=<TARGET-3-IP>
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/ansible_key


[all:vars]

ansible_python_interpreter=/usr/bin/python3
```

---

# Inventory Structure Explanation

## Host Groups

### Web Servers

```ini
[webservers]
```

Contains:

* Web application servers.
* Frontend workloads.

---

### Database Servers

```ini
[dbservers]
```

Contains:

* Database hosts.
* Backend services.

---

## Connection Variables

| Variable                       | Purpose                   |
| ------------------------------ | ------------------------- |
| `ansible_host`                 | Target machine IP address |
| `ansible_user`                 | SSH username              |
| `ansible_ssh_private_key_file` | SSH key location          |
| `ansible_python_interpreter`   | Python path               |

---

# Part 4 — Test Ansible Connectivity

## Step 10 — Ping All Managed Nodes

Run:

```bash id="8m5q2v"
ansible all -i inventory.ini -m ping
```

Expected result:

```text id="4q9m6v"
SUCCESS => {
    "ping": "pong"
}
```

---

## Step 11 — Execute Remote Command

Run:

```bash id="3m7q8p"
ansible webservers -i inventory.ini -m shell -a 'uname -a'
```

This executes:

```bash
uname -a
```

on all hosts in:

```text
webservers
```

group.

---

# Ansible Workflow

```text id="5v9m3q"
Create Controller
        |
        ↓
Install Ansible
        |
        ↓
Configure SSH Keys
        |
        ↓
Create Inventory
        |
        ↓
Test Connectivity
        |
        ↓
Run Automation Tasks
```

---

# Useful Ansible Commands

| Command                    | Purpose                    |
| -------------------------- | -------------------------- |
| `ansible --version`        | Check Ansible installation |
| `ansible all -m ping`      | Test connectivity          |
| `ansible all -m shell -a`  | Execute remote command     |
| `ansible-playbook`         | Run automation playbooks   |
| `ansible-inventory --list` | View inventory             |

---

# Best Practice Tips

> [!TIP]
> Follow these practices when building production Ansible automation.

## ⚙️ Use ansible.cfg

Create:

```text id="9x3m6q"
ansible.cfg
```

inside your project directory.

Benefits:

* Defines default inventory.
* Avoids repeating:

```bash
-i inventory.ini
```

in every command.

---

## 📁 Use Group Variables

Instead of placing variables directly in inventory:

Create:

```text id="7m4q9p"
group_vars/
```

Example:

```text
group_vars/webservers.yml
```

Benefits:

* Cleaner inventory.
* Easier environment management.
* Better scalability.

---

## 🔑 Always Use SSH Keys

Avoid:

```bash
--ask-pass
```

for automation.

Use:

* SSH keys.
* Secure key management.
* Automated authentication.

---

## ☁️ Use Dynamic Inventory in Production

For cloud environments:

Use:

* AWS dynamic inventory.
* Azure dynamic inventory.

Avoid relying only on:

```text
inventory.ini
```

for large production environments.

Benefits:

* Automatically discovers instances.
* Handles scaling.
* Reduces manual updates.

---

# Validation Checklist

Verify:

* ✅ Ansible installed successfully.
* ✅ SSH key pair created.
* ✅ Target nodes configured.
* ✅ Passwordless SSH tested.
* ✅ Inventory file created.
* ✅ Managed nodes added.
* ✅ Ansible ping successful.
* ✅ Remote commands executed.

---

# Summary

In this lab, you:

* Installed Ansible on a controller node.
* Configured SSH-based authentication.
* Created an Ansible inventory.
* Connected to managed nodes.
* Verified Ansible automation readiness.

Your Ansible environment is now ready for creating playbooks and automating infrastructure tasks.
