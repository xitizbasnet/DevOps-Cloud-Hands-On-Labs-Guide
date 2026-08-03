# Section 5 — Kubernetes

# Lab 24 — Create 3 Ubuntu VMs for Kubernetes Cluster (AWS, Azure)

> [!IMPORTANT]
> This lab demonstrates how to provision the infrastructure required for a self-managed Kubernetes cluster by creating **one master node and two worker nodes** using Ubuntu virtual machines on AWS.

## Objective

Provision:

* 🧠 1 Kubernetes Master node.
* ⚙️ 2 Kubernetes Worker nodes.

as Ubuntu VMs on AWS.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ An AWS account.
* ✅ Basic AWS EC2 knowledge.
* ✅ Understanding of Linux administration.
* ✅ SSH access configured.
* ✅ Required permissions to create EC2 resources.

---

# Kubernetes Cluster Architecture

The cluster layout:

```text
                 Kubernetes Cluster

              ┌─────────────────┐
              │  k8s-master     │
              │  Control Plane  │
              └────────┬────────┘
                       |
          ┌────────────┴────────────┐
          |                         |
          ↓                         ↓
 ┌─────────────────┐      ┌─────────────────┐
 │ k8s-worker-1    │      │ k8s-worker-2    │
 │ Worker Node     │      │ Worker Node     │
 └─────────────────┘      └─────────────────┘
```

---

# Procedure

# Part 1 — Create Kubernetes EC2 Instances

## Step 1 — Launch Three EC2 Instances

Launch three Ubuntu virtual machines:

Configuration:

| Setting          | Value               |
| ---------------- | ------------------- |
| Operating System | Ubuntu 22.04        |
| Instance Type    | t2.medium           |
| Network          | Same VPC            |
| Security Group   | Same Security Group |

Create instances with the following names:

| Node Type     | Instance Name  |
| ------------- | -------------- |
| Master Node   | `k8s-master`   |
| Worker Node 1 | `k8s-worker-1` |
| Worker Node 2 | `k8s-worker-2` |

---

# Part 2 — Configure Kubernetes Security Group

## Step 2 — Add Required Inbound Rules

Apply the following inbound rules to all three nodes.

| Protocol    | Port        | Source                | Purpose                  |
| ----------- | ----------- | --------------------- | ------------------------ |
| SSH         | 22          | My IP                 | Server administration    |
| TCP         | 6443        | 0.0.0.0/0             | Kubernetes API Server    |
| TCP         | 2379-2380   | Security Group itself | etcd communication       |
| TCP         | 10250       | Security Group itself | Kubelet API              |
| TCP         | 10251-10252 | Security Group itself | Scheduler / Controller   |
| All Traffic | All         | Security Group itself | Pod-to-pod communication |

---

## Kubernetes Required Ports

### API Server

```text
6443
```

Purpose:

* Kubernetes control plane communication.
* kubectl API access.

---

### etcd

```text
2379-2380
```

Purpose:

* Kubernetes cluster state storage.
* Master node communication.

---

### Kubelet

```text
10250
```

Purpose:

* Node management.
* Container lifecycle communication.

---

### Scheduler and Controller Manager

```text
10251-10252
```

Purpose:

* Kubernetes scheduling.
* Controller operations.

---

# Part 3 — Configure Hostnames

## Step 3 — Configure Master Node Hostname

On the master node:

```bash id="4m8q7v"
sudo hostnamectl set-hostname k8s-master
```

---

## Step 4 — Configure Worker Node 1 Hostname

On worker node 1:

```bash id="8q3n5m"
sudo hostnamectl set-hostname k8s-worker-1
```

---

## Step 5 — Configure Worker Node 2 Hostname

On worker node 2:

```bash id="6p9m2x"
sudo hostnamectl set-hostname k8s-worker-2
```

---

# Part 4 — Configure `/etc/hosts`

## Step 6 — Update Host Resolution on ALL Nodes

On all three nodes:

Edit:

```bash id="2v7n8q"
sudo nano /etc/hosts
```

Add the following entries:

```text id="5m4q9x"
172.31.10.10 k8s-master
172.31.10.11 k8s-worker-1
172.31.10.12 k8s-worker-2
```

> Replace the example private IP addresses with the actual private IP addresses assigned to your EC2 instances.

---

# Node Communication Validation

Verify hostname resolution:

```bash id="9x3m6q"
ping k8s-master
ping k8s-worker-1
ping k8s-worker-2
```

Verify hostname:

```bash id="1m5q8v"
hostname
```

---

# Kubernetes Infrastructure Workflow

```text
Create EC2 Instances
          |
          ↓
Configure Security Group
          |
          ↓
Set Hostnames
          |
          ↓
Configure /etc/hosts
          |
          ↓
Install Kubernetes Components
          |
          ↓
Initialize Cluster
```

---

# Best Practice Tips

> [!TIP]
> Follow these recommendations when building Kubernetes clusters.

## ⚙️ Use Sufficient VM Resources

Use:

```text
t2.medium
```

minimum for Kubernetes nodes.

Specifications:

* 2 vCPU.
* 4 GB RAM.

`t2.micro` instances are insufficient for Kubernetes workloads.

---

## 🌐 Keep Nodes in the Same Network

Place all Kubernetes nodes in:

* Same VPC.
* Same Availability Zone.

Benefits:

* Lower network latency.
* Faster cluster communication.
* Improved reliability.

---

## 🔒 Use Private IP Communication

Use:

```text
Private IP addresses
```

for cluster communication.

Reason:

* Public IP addresses may change after stop/start operations.
* Internal communication should remain stable.

---

## ☁️ Use Managed Kubernetes for Production

For production environments:

Recommended:

```text
Amazon EKS
```

Benefits:

* Managed control plane.
* Automated upgrades.
* Reduced operational overhead.

Self-managed Kubernetes clusters are recommended primarily for:

* Learning.
* Testing.
* Lab environments.

---

# Validation Checklist

Verify that the following tasks have been completed successfully:

* ✅ Three Ubuntu EC2 instances created.
* ✅ Master and worker node names assigned.
* ✅ Security group rules configured.
* ✅ Kubernetes ports opened.
* ✅ Hostnames configured.
* ✅ `/etc/hosts` updated on all nodes.
* ✅ Nodes can communicate using private IPs.

---

# Summary

In this lab, you:

* Provisioned Kubernetes infrastructure on AWS.
* Created one master and two worker nodes.
* Configured required Kubernetes networking ports.
* Assigned cluster hostnames.
* Prepared nodes for Kubernetes installation.

You are now ready to continue with Kubernetes software installation and cluster initialization in the next lab.
