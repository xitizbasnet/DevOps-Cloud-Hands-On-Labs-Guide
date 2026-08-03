# Lab 35 — Creating VPC & VNet Networks Using Terraform

> [!IMPORTANT]
> This lab demonstrates how to build cloud networking infrastructure using Terraform. You will create an **AWS VPC** with public and private subnets, an Internet Gateway, route tables, and subnet associations.

## Objective

Build a complete AWS VPC with:

* 🌐 Virtual Private Cloud (VPC).
* 🌍 Public subnets.
* 🔒 Private subnets.
* 🚪 Internet Gateway (IGW).
* 🛣️ Route tables.
* 🔗 Route table associations.

By the end of this lab, you will understand how Terraform can automate cloud network provisioning.

---

# Prerequisites

Before starting, ensure:

* ✅ Terraform installed and configured.
* ✅ AWS provider configured.
* ✅ AWS credentials available.
* ✅ Availability zone data source configured.
* ✅ Basic AWS networking knowledge.

---

# Network Architecture Overview

The Terraform configuration creates:

```text id="8k4m2p"
                    AWS VPC
                CIDR: 10.0.0.0/16
                       |
        ┌──────────────┴──────────────┐
        |                             |
 Public Subnets              Private Subnets
        |                             |
        ↓                             ↓
 Internet Gateway              Internal Resources
        |
        ↓
 Public Internet
```

---

# Part 1 — Create VPC Infrastructure

## Step 1 — Create Terraform Network File

Create:

```text id="7m3q8v"
vpc.tf
```

Add:

```hcl id="9v5m2q"
resource "aws_vpc" "main" {

  cidr_block = "10.0.0.0/16"

  enable_dns_hostnames = true

  enable_dns_support = true


  tags = {

    Name = "devops-vpc"

  }

}


resource "aws_internet_gateway" "igw" {

  vpc_id = aws_vpc.main.id


  tags = {

    Name = "devops-igw"

  }

}


resource "aws_subnet" "public" {

  count = 2

  vpc_id = aws_vpc.main.id


  cidr_block = "10.0.${count.index}.0/24"


  availability_zone =
    data.aws_availability_zones.available.names[count.index]


  map_public_ip_on_launch = true


  tags = {

    Name = "public-subnet-${count.index + 1}"

  }

}


resource "aws_subnet" "private" {

  count = 2

  vpc_id = aws_vpc.main.id


  cidr_block = "10.0.${count.index + 10}.0/24"


  availability_zone =
    data.aws_availability_zones.available.names[count.index]


  tags = {

    Name = "private-subnet-${count.index + 1}"

  }

}


resource "aws_route_table" "public" {

  vpc_id = aws_vpc.main.id


  route {

    cidr_block = "0.0.0.0/0"

    gateway_id = aws_internet_gateway.igw.id

  }


  tags = {

    Name = "public-rt"

  }

}


resource "aws_route_table_association" "public" {

  count = 2


  subnet_id =
    aws_subnet.public[count.index].id


  route_table_id =
    aws_route_table.public.id

}
```

---

# Part 2 — Configuration Explanation

## VPC Resource

Configuration:

```hcl id="5p9m3x"
resource "aws_vpc" "main"
```

Creates:

```text id="6q8m2v"
VPC CIDR:
10.0.0.0/16
```

Features enabled:

* DNS hostnames.
* DNS resolution.

---

# Internet Gateway

Resource:

```hcl id="3m7q8p"
aws_internet_gateway
```

Purpose:

* Provides internet connectivity.
* Allows public subnet resources to communicate externally.

Traffic flow:

```text id="4n8m2q"
EC2 Instance
      |
Public Subnet
      |
Internet Gateway
      |
Internet
```

---

# Public Subnets

Configuration:

```hcl id="7x3m9p"
resource "aws_subnet" "public"
```

Creates:

```text id="8q5m2v"
2 Public Subnets
```

Features:

* Distributed across availability zones.
* Automatically assigns public IP addresses.

Configuration:

```hcl id="2m6q8v"
map_public_ip_on_launch = true
```

---

# Private Subnets

Configuration:

```hcl id="5v8m3q"
resource "aws_subnet" "private"
```

Creates:

```text id="9m4q7x"
2 Private Subnets
```

Characteristics:

* No direct internet access.
* Used for internal workloads.
* Suitable for application and database tiers.

---

# Public Route Table

Resource:

```hcl id="6p2m8v"
aws_route_table.public
```

Route:

```text id="3q9m5x"
0.0.0.0/0
        |
        ↓
Internet Gateway
```

Allows public subnet resources to access the internet.

---

# Route Table Association

Resource:

```hcl id="8m3q6v"
aws_route_table_association
```

Purpose:

* Connects public subnets to public route table.
* Enables internet routing.

---

# Part 3 — Deploy Network Infrastructure

## Step 2 — Initialize Terraform

Run:

```bash id="5x7m2q"
terraform init
```

---

## Step 3 — Validate Configuration

Run:

```bash id="8v3m5p"
terraform validate
```

---

## Step 4 — Review Network Plan

Run:

```bash id="2q6m9v"
terraform plan
```

Review:

* VPC creation.
* Subnet creation.
* Gateway creation.
* Route configuration.

---

## Step 5 — Deploy VPC

Run:

```bash id="7m4q8p"
terraform apply
```

Terraform creates:

* AWS VPC.
* Internet Gateway.
* Public subnets.
* Private subnets.
* Route tables.

---

# Network Resource Summary

| Resource                      | Purpose                         |
| ----------------------------- | ------------------------------- |
| `aws_vpc`                     | Creates virtual network         |
| `aws_internet_gateway`        | Provides internet connectivity  |
| `aws_subnet`                  | Creates public/private networks |
| `aws_route_table`             | Defines traffic routing         |
| `aws_route_table_association` | Connects subnets to routes      |

---

# Public vs Private Subnet Design

| Feature           | Public Subnet               | Private Subnet          |
| ----------------- | --------------------------- | ----------------------- |
| Internet access   | Yes                         | No direct access        |
| Public IP         | Enabled                     | Disabled                |
| Typical workloads | Load balancers, web servers | Applications, databases |
| Security exposure | Higher                      | Lower                   |

---

# Best Practice Tips

> [!TIP]
> Follow these recommendations when designing cloud networks.

---

## 🌍 Deploy Across Multiple Availability Zones

Always use:

```text id="4x8m2q"
2+ Availability Zones
```

Benefits:

* High availability.
* Fault tolerance.
* Better resilience.

Avoid:

```text id="6m9q3v"
Single AZ VPC deployments
```

for production workloads.

---

## 🔒 Use Private Subnets for Application Servers

Recommended architecture:

```text id="8q2m5v"
Internet
   |
Load Balancer
   |
Public Subnet
   |
Private Application Servers
   |
Private Database Layer
```

Avoid placing application servers directly in public subnets.

---

## 📦 Use Terraform Modules for VPC

Recommended community module:

```text id="3m7q9v"
terraform-aws-modules/vpc/aws
```

Benefits:

* Standardized networking.
* Reduced configuration errors.
* Production-ready patterns.

---

## 📊 Enable VPC Flow Logs

Send logs to:

```text id="5q8m2v"
S3
CloudWatch
```

Benefits:

* Security auditing.
* Network troubleshooting.
* Traffic analysis.

---

# Validation Checklist

Verify:

* ✅ VPC created successfully.
* ✅ CIDR block configured.
* ✅ Internet Gateway attached.
* ✅ Two public subnets created.
* ✅ Two private subnets created.
* ✅ Public route table configured.
* ✅ Subnets associated correctly.
* ✅ Terraform deployment completed successfully.

---

# Summary

In this lab, you:

* Created an AWS VPC using Terraform.
* Built public and private subnet architecture.
* Configured Internet Gateway connectivity.
* Created route tables.
* Learned production network design practices.

This foundation can be extended for enterprise workloads using load balancers, NAT gateways, Kubernetes clusters, and multi-tier applications.
