# Lab 25 — Install & Configure 3-Node Kubernetes Cluster with Kubeadm

> [!IMPORTANT]
> This lab demonstrates how to install and configure a self-managed Kubernetes cluster using **kubeadm**. You will install the container runtime, Kubernetes components, initialize the control plane, configure networking, and join worker nodes.

## Objective

Install and configure:

* 📦 containerd — Container runtime.
* ⚙️ kubeadm — Kubernetes cluster bootstrap tool.
* 🔧 kubelet — Node agent.
* 🖥️ kubectl — Kubernetes command-line tool.

Bootstrap a:

* 🧠 1 Master node.
* ⚙️ 2 Worker nodes.

Kubernetes cluster.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 24 — Create 3 Ubuntu VMs for Kubernetes Cluster**.
* ✅ Three Ubuntu 22.04 nodes available.
* ✅ SSH access to all nodes.
* ✅ Private IP communication configured.
* ✅ Security group rules configured.

Cluster nodes:

| Node Role     | Hostname       |
| ------------- | -------------- |
| Control Plane | `k8s-master`   |
| Worker Node   | `k8s-worker-1` |
| Worker Node   | `k8s-worker-2` |

---

# Kubernetes Cluster Installation Flow

```text
Disable Swap
      |
      ↓
Configure Kernel Modules
      |
      ↓
Install containerd
      |
      ↓
Install kubeadm/kubelet/kubectl
      |
      ↓
Initialize Master Node
      |
      ↓
Install CNI Network Plugin
      |
      ↓
Join Worker Nodes
      |
      ↓
Verify Cluster
```

---

# Procedure

# Part 1 — Configure Operating System on All Nodes

> Perform all commands in this section on:
>
> * k8s-master
> * k8s-worker-1
> * k8s-worker-2

---

## Step 1 — Disable Swap

Kubernetes requires swap to be disabled.

Run:

```bash
sudo swapoff -a
```

Disable swap permanently:

```bash
sudo sed -i '/ swap / s/^/#/' /etc/fstab
```

Verify:

```bash
free -h
```

---

## Step 2 — Load Required Kernel Modules

Create Kubernetes module configuration:

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF
```

Load modules:

```bash
sudo modprobe overlay
sudo modprobe br_netfilter
```

---

## Step 3 — Configure Kernel Parameters

Create sysctl configuration:

```bash
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF
```

Apply settings:

```bash
sudo sysctl --system
```

---

# Part 2 — Install containerd

> Perform on all three nodes.

---

## Step 4 — Install containerd Package

Update packages:

```bash
sudo apt update
```

Install containerd:

```bash
sudo apt install -y containerd
```

---

## Step 5 — Configure containerd

Create configuration directory:

```bash
sudo mkdir -p /etc/containerd
```

Generate default configuration:

```bash
containerd config default | sudo tee /etc/containerd/config.toml
```

Enable systemd cgroup driver:

```bash
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml
```

Restart and enable containerd:

```bash
sudo systemctl restart containerd
```

```bash
sudo systemctl enable containerd
```

Verify:

```bash
systemctl status containerd
```

---

# Part 3 — Install Kubernetes Components

> Perform on all three nodes.

---

## Step 6 — Install Required Packages

Install dependencies:

```bash
sudo apt install -y apt-transport-https ca-certificates curl
```

---

## Step 7 — Add Kubernetes Repository Key

Download Kubernetes signing key:

```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | \
sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

---

## Step 8 — Add Kubernetes Repository

Add repository:

```bash
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] \
https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | \
sudo tee /etc/apt/sources.list.d/kubernetes.list
```

---

## Step 9 — Install kubeadm, kubelet, kubectl

Update packages:

```bash
sudo apt update
```

Install Kubernetes tools:

```bash
sudo apt install -y kubelet kubeadm kubectl
```

Prevent automatic upgrades:

```bash
sudo apt-mark hold kubelet kubeadm kubectl
```

---

# Part 4 — Initialize Kubernetes Master Node

> Perform only on:
>
> `k8s-master`

---

## Step 10 — Initialize Cluster

Run:

```bash
sudo kubeadm init \
--pod-network-cidr=192.168.0.0/16 \
--apiserver-advertise-address=<MASTER-PRIVATE-IP>
```

Replace:

```text
<MASTER-PRIVATE-IP>
```

with the private IP address of the master node.

Example:

```bash
--apiserver-advertise-address=172.31.10.10
```

---

# Part 5 — Configure kubectl Access

## Step 11 — Configure kubectl for Ubuntu User

Create Kubernetes configuration directory:

```bash
mkdir -p $HOME/.kube
```

Copy admin configuration:

```bash
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
```

Change ownership:

```bash
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Verify:

```bash
kubectl get nodes
```

---

# Part 6 — Install Calico CNI Network Plugin

## Step 12 — Deploy Calico

Install Calico networking:

```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml
```

Calico provides:

* Pod networking.
* Network policies.
* Container communication.

---

# Part 7 — Generate Worker Join Command

## Step 13 — Create Join Command

Generate worker node join command:

```bash
kubeadm token create --print-join-command
```

Example output:

```bash
sudo kubeadm join <MASTER-IP>:6443 \
--token <TOKEN> \
--discovery-token-ca-cert-hash sha256:<HASH>
```

> Save this command. It is required for worker nodes.

---

# Part 8 — Join Worker Nodes

> Perform on:
>
> * k8s-worker-1
> * k8s-worker-2

---

## Step 14 — Join Kubernetes Cluster

Run the join command generated from the master node:

```bash
sudo kubeadm join <MASTER-IP>:6443 --token <TOKEN> --discovery-token-ca-cert-hash sha256:<HASH>
```

Successful output confirms:

```text
This node has joined the cluster
```

---

# Part 9 — Verify Kubernetes Cluster

> Perform on:
>
> `k8s-master`

---

## Step 15 — Check Nodes

Run:

```bash
kubectl get nodes
```

Expected output:

```text
NAME            STATUS   ROLES
k8s-master      Ready    control-plane
k8s-worker-1    Ready    <none>
k8s-worker-2    Ready    <none>
```

---

## Step 16 — Check System Pods

Run:

```bash
kubectl get pods -n kube-system
```

Verify:

* Calico pods are running.
* Core Kubernetes services are healthy.

---

# Kubernetes Cluster Validation Checklist

Verify:

* ✅ Swap disabled on all nodes.
* ✅ Kernel modules configured.
* ✅ containerd installed and running.
* ✅ kubeadm, kubelet, kubectl installed.
* ✅ Master node initialized.
* ✅ kubectl configured.
* ✅ Calico CNI installed.
* ✅ Worker nodes joined.
* ✅ All nodes show Ready status.

---

# Best Practice Tips

> [!TIP]
> Follow these practices when operating kubeadm-based Kubernetes clusters.

## 🔑 Save kubeadm Join Command

The join token:

* Expires after 24 hours.
* Must be regenerated if expired.

Generate a new command:

```bash
kubeadm token create --print-join-command
```

---

## ⏳ Wait After Installing Calico

After installing Calico:

* Wait 2–3 minutes.
* Check node readiness.

Command:

```bash
kubectl get nodes
```

Nodes should become:

```text
Ready
```

---

## 🗂️ Use Kubernetes Contexts

For multiple clusters:

```bash
kubectl config set-context
```

Benefits:

* Safer cluster management.
* Prevents accidental commands against the wrong cluster.

---

## 👤 Avoid Running kubectl as Root

Do not use:

```bash
sudo kubectl
```

Instead configure:

```text
~/.kube/config
```

for your normal user.

Benefits:

* Better security.
* Proper user permissions.
* Standard Kubernetes administration practice.

---

# Summary

In this lab, you:

* Installed containerd on Kubernetes nodes.
* Installed kubeadm, kubelet, and kubectl.
* Initialized a Kubernetes control plane.
* Installed Calico networking.
* Joined worker nodes to the cluster.
* Verified Kubernetes cluster health.

Your 3-node Kubernetes cluster is now ready for application deployment.
