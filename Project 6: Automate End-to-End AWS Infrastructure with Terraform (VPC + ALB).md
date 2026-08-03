# Project 6: Automate End-to-End AWS Infrastructure with Terraform (VPC + ALB)

## Overview

This project demonstrates the automation of an **end-to-end AWS infrastructure deployment** using **Terraform**.

The solution provisions a highly available application infrastructure architecture consisting of:

* AWS Virtual Private Cloud (VPC)
* Public Subnets
* Application Load Balancer (ALB)
* Target Group
* Auto Scaling Group (ASG)
* EC2 Launch Template

The infrastructure follows cloud-native design principles by providing scalability, availability, and automated instance management.

---

# 🛠️ Tools and Technologies

| Tool / Technology                  | Purpose                                                                |
| ---------------------------------- | ---------------------------------------------------------------------- |
| 🏗️ Terraform                      | Automates AWS infrastructure provisioning using Infrastructure as Code |
| ☁️ AWS VPC                         | Provides isolated networking environment                               |
| ⚖️ Application Load Balancer (ALB) | Distributes incoming application traffic                               |
| 🔄 Auto Scaling Group (ASG)        | Automatically manages EC2 instance scaling                             |
| 🚀 Launch Template                 | Defines EC2 instance configuration                                     |

---

# 🏛️ Architecture Overview

The deployed AWS architecture follows this flow:

```text
VPC
 │
 ├── Public Subnet 1
 │
 ├── Public Subnet 2
 │
 ▼
Application Load Balancer (ALB)
 │
 ▼
Target Group
 │
 ▼
Auto Scaling Group (ASG)
 │
 ├── EC2 Instance
 │
 └── EC2 Instance

Scaling Configuration:
Minimum Instances: 2
Maximum Instances: 5
Desired Instances: 2
```

---

# 🚀 Infrastructure Deployment Workflow

The Terraform deployment performs the following actions:

1. Creates an AWS VPC.
2. Creates two public subnets across availability zones.
3. Creates an Application Load Balancer.
4. Creates an ALB Target Group.
5. Creates an EC2 Launch Template.
6. Creates an Auto Scaling Group.
7. Registers EC2 instances with the Target Group.
8. Enables automatic scaling between minimum and maximum capacity.

---

# 📁 Terraform Configuration

## Key AWS Resources

The following Terraform resources define the core infrastructure components.

---

# ⚖️ Application Load Balancer (ALB)

## Resource: `aws_lb`

The Application Load Balancer provides external access and distributes incoming traffic across healthy application servers.

```hcl id="r5m7px"
resource "aws_lb" "main" {

  name = "devops-alb"

  internal = false

  load_balancer_type = "application"

  security_groups = [
    aws_security_group.alb_sg.id
  ]

  subnets = aws_subnet.public[*].id

}
```

---

## ALB Configuration Details

| Configuration      | Value                          |
| ------------------ | ------------------------------ |
| Load Balancer Name | `devops-alb`                   |
| Type               | Application Load Balancer      |
| Internal           | `false` (Internet-facing)      |
| Security Group     | `aws_security_group.alb_sg.id` |
| Subnets            | Public Subnets                 |

---

# 🎯 Target Group

## Purpose

The Target Group connects the Application Load Balancer with backend EC2 application servers.

Traffic flow:

```text
Client Request
      |
      ▼
Application Load Balancer
      |
      ▼
Target Group
      |
      ▼
EC2 Instances
```

The Auto Scaling Group automatically registers and removes EC2 instances from the Target Group.

---

# 🔄 Auto Scaling Group (ASG)

## Resource: `aws_autoscaling_group`

The Auto Scaling Group automatically manages EC2 application servers based on configured capacity settings.

```hcl id="1uvxj9"
resource "aws_autoscaling_group" "app" {

  name = "devops-asg"

  vpc_zone_identifier = aws_subnet.private[*].id

  target_group_arns = [
    aws_lb_target_group.app.arn
  ]

  min_size = 2

  max_size = 5

  desired_capacity = 2


  launch_template {

    id = aws_launch_template.app.id

    version = "$Latest"

  }


  tag {

    key = "Name"

    value = "devops-app-server"

    propagate_at_launch = true

  }

}
```

---

## Auto Scaling Configuration Details

| Configuration     | Value                     |
| ----------------- | ------------------------- |
| ASG Name          | `devops-asg`              |
| Minimum Capacity  | `2` EC2 instances         |
| Maximum Capacity  | `5` EC2 instances         |
| Desired Capacity  | `2` EC2 instances         |
| Network Placement | Private Subnets           |
| Launch Template   | `aws_launch_template.app` |
| Instance Tag      | `devops-app-server`       |

---

# 🚀 Launch Template

## Purpose

The Launch Template defines the EC2 instance configuration used by the Auto Scaling Group.

It typically contains:

* AMI ID
* Instance type
* Security groups
* User data scripts
* Storage configuration
* Instance metadata options

Example architecture:

```text
Launch Template
        |
        ▼
Auto Scaling Group
        |
        ▼
EC2 Instances
```

---

# 🔐 Network Security Design

Recommended architecture:

```text
Internet
   |
   ▼
Public Subnets
   |
   ▼
Application Load Balancer
   |
   ▼
Private Subnets
   |
   ▼
EC2 Application Servers
```

Benefits:

* Application servers are not directly exposed to the internet.
* ALB handles public traffic.
* Security groups control access between layers.

---

# 📊 High Availability Design

The architecture provides high availability through:

* Multiple public subnets.
* Application Load Balancer distribution.
* Auto Scaling Group management.
* Multiple EC2 instances.

> [!IMPORTANT]
> Deploy resources across multiple Availability Zones to reduce downtime caused by single-zone failures.

---

# ✅ Best Practice Recommendations

## Use Infrastructure as Code

Manage AWS infrastructure using Terraform.

Benefits:

* Consistent deployments.
* Version-controlled changes.
* Repeatable environments.
* Easier disaster recovery.

---

## Use Auto Scaling Instead of Fixed Servers

Configure ASG capacity based on application requirements.

Benefits:

* Automatic scaling.
* Improved availability.
* Better resource utilization.

---

## Use Private Subnets for Application Servers

Keep backend EC2 instances private.

Benefits:

* Reduces security exposure.
* Enforces controlled access through ALB.
* Follows AWS security best practices.

---

## Enable ALB Health Checks

Configure Target Group health checks.

Benefits:

* Removes unhealthy instances automatically.
* Routes traffic only to healthy servers.
* Improves application reliability.

---

# 📚 Summary

This Terraform-based AWS infrastructure project creates a complete production-style application environment using:

* 🌐 AWS VPC networking.
* ⚖️ Application Load Balancer.
* 🎯 Target Group routing.
* 🚀 EC2 Launch Template.
* 🔄 Auto Scaling Group.

The architecture provides:

* High availability.
* Automated scaling.
* Secure network separation.
* Repeatable infrastructure deployment.

This implementation follows modern DevOps and cloud infrastructure automation practices using Terraform on AWS.
