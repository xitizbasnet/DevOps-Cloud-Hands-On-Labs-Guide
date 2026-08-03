# Lab 33 — Create EC2 Instance on AWS & VM on Azure Using Terraform

> [!IMPORTANT]
> This lab demonstrates how to provision cloud infrastructure using Terraform. You will create an **AWS EC2 instance** with a key pair, security group, encrypted EBS storage, and automated Nginx installation using `user_data`.

## Objective

Provision EC2 with:

* 🔑 Key pair authentication.
* 🔒 Security Group configuration.
* 💾 EBS storage.
* ⚙️ Automated instance bootstrap.
* 🌐 Web server deployment using Terraform.

By the end of this lab, you will understand how Terraform can automate AWS infrastructure provisioning.

---

# Prerequisites

Before starting, ensure:

* ✅ Terraform installed and configured.
* ✅ AWS provider configured.
* ✅ AWS CLI authentication completed.
* ✅ SSH key pair available.
* ✅ AWS region configured.

---

# Terraform Infrastructure Overview

The Terraform workflow will create:

```text
Terraform Configuration
          |
          ↓
    AWS Provider
          |
 ┌────────┼────────┐
 ↓        ↓        ↓
EC2    Security   EBS
VM      Group    Volume
          |
          ↓
     Nginx Web Server
```

---

# Part 1 — Create AWS EC2 Infrastructure

## Step 1 — Create Terraform Configuration

Create file:

```text
ec2.tf
```

Add:

```hcl
data "aws_ami" "ubuntu" {

  most_recent = true

  owners = ["099720109477"]

  filter {

    name = "name"

    values = [
      "ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"
    ]

  }

}


resource "aws_key_pair" "devops_key" {

  key_name = "devops-tf-key"

  public_key = file("~/.ssh/id_ed25519.pub")

}


resource "aws_security_group" "web_sg" {

  name = "web-sg-tf"

  description = "Web server security group"


  ingress {

    from_port = 22

    to_port = 22

    protocol = "tcp"

    cidr_blocks = [
      "0.0.0.0/0"
    ]

  }


  ingress {

    from_port = 80

    to_port = 80

    protocol = "tcp"

    cidr_blocks = [
      "0.0.0.0/0"
    ]

  }


  egress {

    from_port = 0

    to_port = 0

    protocol = "-1"

    cidr_blocks = [
      "0.0.0.0/0"
    ]

  }


  tags = {

    Name = "web-sg-tf"

  }

}


resource "aws_instance" "web" {

  ami = data.aws_ami.ubuntu.id

  instance_type = "t2.micro"

  key_name = aws_key_pair.devops_key.key_name


  vpc_security_group_ids = [
    aws_security_group.web_sg.id
  ]


  root_block_device {

    volume_size = 10

    volume_type = "gp3"

    encrypted = true

  }


  user_data = <<-EOF

#!/bin/bash

apt update && apt install -y nginx

systemctl start nginx

systemctl enable nginx

EOF


  tags = {

    Name = "tf-web-server"

    Environment = "dev"

    ManagedBy = "terraform"

  }

}


output "instance_public_ip" {

  value = aws_instance.web.public_ip

}
```

---

# Configuration Explanation

## AMI Data Source

```hcl
data "aws_ami" "ubuntu"
```

Purpose:

* Finds the latest Ubuntu 22.04 AMI.
* Avoids manually managing AMI IDs.
* Automatically selects the correct image.

---

## AWS Key Pair

```hcl
resource "aws_key_pair" "devops_key"
```

Creates:

```text
devops-tf-key
```

Uses:

```text
~/.ssh/id_ed25519.pub
```

for SSH authentication.

---

## Security Group

The security group allows:

| Port | Protocol | Purpose          |
| ---- | -------- | ---------------- |
| 22   | TCP      | SSH access       |
| 80   | TCP      | HTTP web traffic |

Outbound traffic:

```text
All traffic allowed
```

---

## EC2 Instance

Terraform creates:

```text
Instance Type:
t2.micro
```

Configuration:

* Ubuntu 22.04.
* SSH key authentication.
* Security group attached.
* Encrypted EBS storage.

---

## EBS Root Volume

Configuration:

```hcl
root_block_device {

  volume_size = 10

  volume_type = "gp3"

  encrypted = true

}
```

Creates:

* 10 GB storage.
* gp3 volume type.
* Encryption enabled.

---

## User Data Bootstrap

Terraform automatically runs:

```bash
apt update
apt install -y nginx
systemctl start nginx
systemctl enable nginx
```

Purpose:

* Installs Nginx automatically.
* Starts web service.
* Enables startup after reboot.

---

# Part 2 — Deploy Infrastructure

## Step 2 — Initialize Terraform

```bash
terraform init
```

Downloads:

* AWS provider.
* Required Terraform plugins.

---

## Step 3 — Validate Configuration

```bash
terraform validate
```

Checks:

* Terraform syntax.
* Configuration correctness.

---

## Step 4 — Review Plan

```bash
terraform plan
```

Displays:

* Resources to create.
* Configuration changes.

---

## Step 5 — Create AWS Resources

Run:

```bash
terraform apply
```

Terraform creates:

* EC2 instance.
* Security Group.
* Key Pair.
* Encrypted EBS volume.

---

# Part 3 — Verify Deployment

## Step 6 — Retrieve Public IP

Run:

```bash
terraform output -raw instance_public_ip
```

Example:

```text
54.xxx.xxx.xxx
```

---

## Step 7 — Test Nginx Web Server

Run:

```bash
curl http://$(terraform output -raw instance_public_ip)
```

Expected:

```text
Nginx welcome page response
```

---

# Terraform Resource Summary

| Resource             | Purpose                         |
| -------------------- | ------------------------------- |
| `aws_ami`            | Finds Ubuntu image              |
| `aws_key_pair`       | Enables SSH access              |
| `aws_security_group` | Controls network access         |
| `aws_instance`       | Creates EC2 server              |
| `root_block_device`  | Configures EBS storage          |
| `user_data`          | Automates software installation |
| `output`             | Displays instance IP            |

---

# Best Practice Tips

> [!TIP]
> Follow these recommendations for production Terraform deployments.

---

## 🖼️ Use Data Sources for AMIs

Avoid:

```text
Hardcoded AMI IDs
```

Reason:

* AMI IDs differ between regions.
* Images are updated frequently.
* Data sources automatically discover valid images.

---

## 🔐 Always Encrypt EBS Volumes

Use:

```hcl
encrypted = true
```

Benefits:

* Protects stored data.
* Meets security compliance requirements.
* Prevents unauthorized disk access.

---

## 🏷️ Tag Every Resource

Recommended tags:

```text
Environment
ManagedBy=terraform
Owner
```

Benefits:

* Resource tracking.
* Cost management.
* Ownership identification.

---

## ⚙️ Use user_data for Bootstrap

Use:

```text
user_data
```

for:

* Package installation.
* Initial configuration.
* Service setup.

For advanced workflows:

```text
user_data + null_resource
```

can handle post-provisioning tasks.

---

# Validation Checklist

Verify:

* ✅ Terraform initialized successfully.
* ✅ Ubuntu AMI discovered dynamically.
* ✅ Key pair created.
* ✅ Security group configured.
* ✅ EC2 instance deployed.
* ✅ EBS encryption enabled.
* ✅ Nginx installed automatically.
* ✅ Web server accessible.

---

# Summary

In this lab, you:

* Created an AWS EC2 instance using Terraform.
* Configured SSH key authentication.
* Created security rules.
* Added encrypted EBS storage.
* Automated Nginx installation.
* Verified the deployed web server.

Terraform can now be used to repeatably create AWS infrastructure using Infrastructure as Code (IaC).
