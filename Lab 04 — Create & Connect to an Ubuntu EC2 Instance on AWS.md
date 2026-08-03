# Lab 04 — Create & Connect to an Ubuntu EC2 Instance on AWS

> [!IMPORTANT]
> This lab demonstrates how to launch an **Ubuntu Server 22.04 LTS** Amazon EC2 instance in the **ap-south-1 (Mumbai)** Region, configure secure network access using **Security Groups**, and connect to the instance via SSH.

## Objective

Launch an Ubuntu EC2 instance in **ap-south-1**, configure **Security Groups**, and connect via **SSH**.

## Prerequisites

Before you begin, ensure that you have:

* ✅ An active AWS Free Tier account.
* ✅ AWS Management Console access.
* ✅ The AWS Region set to **ap-south-1 (Mumbai)**.
* ✅ An SSH client installed (Linux, macOS, or Windows with OpenSSH).

> [!NOTE]
> This lab uses the **t2.micro** instance type and **Ubuntu Server 22.04 LTS (Free Tier eligible)** AMI.

---

# Procedure

## Step 1 — Launch an EC2 Instance

1. Sign in to the **AWS Management Console**.
2. Navigate to:

   ```text
   EC2 → Launch Instance
   ```

---

## Step 2 — Configure the Instance

Configure the instance using the following settings:

| Setting                        | Value                                        |
| ------------------------------ | -------------------------------------------- |
| **Name**                       | `devops-lab-ec2`                             |
| **Amazon Machine Image (AMI)** | Ubuntu Server 22.04 LTS (Free Tier eligible) |

---

## Step 3 — Select the Instance Type and Key Pair

Configure the following options:

| Setting           | Value            |
| ----------------- | ---------------- |
| **Instance Type** | `t2.micro`       |
| **Key Pair**      | Create new       |
| **Key Pair Name** | `devops-lab-key` |

Download the generated **`.pem`** file and store it securely.

> [!WARNING]
> The private key (`.pem`) can only be downloaded once. Store it in a secure location and never share it.

---

## Step 4 — Configure Networking

Configure the networking options as follows:

| Setting                   | Value       |
| ------------------------- | ----------- |
| **VPC**                   | Default VPC |
| **Auto-assign Public IP** | Enabled     |

---

## Step 5 — Configure the Security Group

Create a new Security Group with the following inbound rule:

| Protocol | Port | Source    |
| -------- | ---- | --------- |
| SSH      | 22   | **My IP** |

> [!IMPORTANT]
> Restrict SSH access to **My IP** to minimize unauthorized access.

---

## Step 6 — Configure Storage

Use the default storage configuration:

| Setting         | Value |
| --------------- | ----- |
| **Volume Size** | 8 GB  |
| **Volume Type** | `gp3` |

Select **Launch Instance**.

---

## Step 7 — Verify Instance Status

1. Wait until the instance status displays:

   ```text
   2/2 Status Checks Passed
   ```

2. Copy the instance **Public IPv4 Address** from the EC2 console.

---

## Step 8 — Connect to the EC2 Instance

Set the appropriate permissions on the private key:

```bash
chmod 400 ~/devops-lab-key.pem
```

Connect to the EC2 instance:

```bash
ssh -i ~/devops-lab-key.pem ubuntu@<PUBLIC_IP>
```

Replace:

```text
<PUBLIC_IP>
```

with the Public IPv4 address of your EC2 instance.

### Verify the Connection

Run the following commands to verify that you have successfully connected:

```bash
uname -a
whoami
df -h
```

---

# Best Practice Tips

> [!TIP]
> Apply the following recommendations to improve the security, reliability, and operational management of your EC2 instances.

## 🔒 Restrict SSH Access

* Always configure the SSH Security Group rule to allow access only from **My IP**.
* **Never** use:

```text
0.0.0.0/0
```

for SSH access in production environments.

---

## 🌐 Use EC2 Instance Connect

If SSH connectivity fails:

* Use **EC2 Instance Connect** from the AWS Console as a browser-based alternative to access your instance.

---

## 💰 Optimize Costs

When the instance is not required:

* **Stop** the instance instead of terminating it.
* Stopping the instance preserves:

  * The attached EBS volume.
  * Your instance configuration.
  * The existing IP assignment (where applicable).

---

## 📊 Enable Detailed Monitoring

For learning and troubleshooting purposes, enable detailed monitoring:

```text
EC2 → Actions → Monitor and Troubleshoot → Enable Detailed Monitoring
```

This provides more granular CloudWatch metrics for your instance.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Ubuntu Server 22.04 LTS EC2 instance created.
* ✅ SSH key pair generated and downloaded.
* ✅ Security Group configured with SSH access from **My IP**.
* ✅ EC2 instance passed **2/2 status checks**.
* ✅ Successfully connected to the EC2 instance using SSH.
* ✅ Verification commands (`uname -a`, `whoami`, and `df -h`) executed successfully.

---

# Summary

In this lab, you:

* Launched an Ubuntu EC2 instance in AWS.
* Configured secure network access using a Security Group.
* Connected securely using SSH.
* Verified the operating system and storage configuration.
* Learned AWS best practices for SSH security, monitoring, and cost optimization.

You are now ready to continue with the next lab in the **Cloud Basics** learning path.
