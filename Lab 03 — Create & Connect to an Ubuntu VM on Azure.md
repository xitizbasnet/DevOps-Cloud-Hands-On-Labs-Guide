# Lab 03 — Create & Connect to an Ubuntu VM on Azure

> [!IMPORTANT]
> This lab demonstrates how to deploy an **Ubuntu 22.04 LTS** virtual machine (VM) on Microsoft Azure and securely connect to it using **SSH public key authentication**.

## Objective

Launch an **Ubuntu 22.04 LTS** VM on Azure and connect to it via SSH.

## Prerequisites

Before you begin, ensure that you have:

* ✅ An active Azure subscription (Free Trial or Pay-As-You-Go).
* ✅ Access to the Azure Portal.
* ✅ A terminal with SSH installed (Linux, macOS, or Windows with OpenSSH).
* ✅ Permission to create Azure Virtual Machines.

> [!NOTE]
> This lab uses the **Standard_B1s** VM size, which is commonly eligible for Azure Free Trial usage (subject to current Azure subscription terms and regional availability).

---

## Procedure

### Step 1 — Create a Virtual Machine

1. Sign in to the **Azure Portal**.
2. Navigate to:

   ```text
   Virtual Machines → Create → Azure Virtual Machine
   ```

---

### Step 2 — Configure the Basic Settings

Configure the VM with the following values:

| Setting                  | Value                     |
| ------------------------ | ------------------------- |
| **Resource Group**       | `DevOps-RG`               |
| **Virtual Machine Name** | `ubuntu-vm-01`            |
| **Region**               | `South India`             |
| **Image**                | `Ubuntu Server 22.04 LTS` |

---

### Step 3 — Select the Virtual Machine Size

Choose the following VM size:

| Setting            | Value                                  |
| ------------------ | -------------------------------------- |
| **Size**           | `Standard_B1s`                         |
| **Specifications** | 1 vCPU, 1 GB RAM (Free Trial eligible) |

---

### Step 4 — Configure Authentication

1. Select:

   ```text
   Authentication Type → SSH Public Key
   ```

2. Choose:

   ```text
   Generate New Key Pair
   ```

3. Download the generated **`.pem`** private key file when prompted.

> [!WARNING]
> Store the downloaded private key securely. It cannot be downloaded again after deployment.

---

### Step 5 — Configure Network Access

Under **Inbound Port Rules**, allow:

```text
SSH (Port 22)
```

---

### Step 6 — Deploy the Virtual Machine

1. Select:

   ```text
   Review + Create
   ```

2. Validate the configuration.

3. Select **Create**.

4. Download the private key if prompted.

---

### Step 7 — Retrieve the Public IP Address

After deployment completes:

1. Open the **Virtual Machine Overview** page.
2. Copy the assigned **Public IP Address**.

---

### Step 8 — Connect to the Virtual Machine

Set the correct permissions on the private key:

```bash
chmod 400 ~/Downloads/ubuntu-vm-01_key.pem
```

Connect to the VM using SSH:

```bash
ssh -i ~/Downloads/ubuntu-vm-01_key.pem azureuser@<PUBLIC_IP>
```

Replace:

```text
<PUBLIC_IP>
```

with the Public IP address copied from the Azure Portal.

---

## Best Practice Tips

> [!TIP]
> Follow these recommendations to improve the security and manageability of your Azure virtual machines.

### 🔐 Secure Your SSH Keys

* Store **`.pem`** files in:

  ```text
  ~/.ssh/
  ```

* Set the appropriate file permissions:

  ```bash
  chmod 400 ~/.ssh/your-key.pem
  ```

* **Never commit private keys to Git repositories.**

---

### 🛡️ Use Azure Bastion in Production

Instead of exposing **SSH (Port 22)** directly to the internet:

* Use **Azure Bastion** for secure browser-based SSH access.
* This reduces the attack surface and improves security.

---

### 💰 Reduce Azure Costs

After completing each lab session, deallocate the VM to avoid unnecessary charges:

```bash
az vm deallocate -g DevOps-RG -n ubuntu-vm-01
```

---

### 🌐 Use a Static Public IP

If you plan to reconnect to the VM multiple times:

* Assign a **Static Public IP Address**.
* This prevents the IP address from changing after the VM is stopped and started.

---

## Validation

Verify that the following tasks have been completed successfully:

* ✅ Ubuntu 22.04 LTS VM deployed.
* ✅ SSH key pair generated and downloaded.
* ✅ Public IP address obtained.
* ✅ SSH connection established successfully.
* ✅ VM is accessible from the terminal.
* ✅ Private key stored securely with the correct file permissions.

---

## Summary

In this lab, you:

* Created an Azure Ubuntu 22.04 LTS virtual machine.
* Configured SSH public key authentication.
* Connected securely using SSH.
* Learned best practices for SSH key management, VM security, and Azure cost optimization.

You are now ready to continue with the next lab in the **Cloud Basics** learning path.
