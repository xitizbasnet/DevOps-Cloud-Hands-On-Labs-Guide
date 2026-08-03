# Project 5: Automate Azure Infrastructure with Terraform (VNet + Subnet + Load Balancer)

## Overview

This project demonstrates how to automate Azure infrastructure provisioning using **Terraform** and the **Azure Provider**.

The Terraform configuration creates the foundational Azure networking components required for a cloud environment:

* Resource Group
* Virtual Network (VNet)
* Subnet
* Azure Load Balancer

Infrastructure is deployed using Infrastructure as Code (IaC), allowing consistent, repeatable, and version-controlled Azure deployments.

---

# 🛠️ Tools and Technologies

| Tool / Technology               | Purpose                                                                  |
| ------------------------------- | ------------------------------------------------------------------------ |
| 🏗️ Terraform                   | Automates Azure infrastructure provisioning using Infrastructure as Code |
| ☁️ Azure Provider for Terraform | Enables Terraform to create and manage Azure resources                   |

---

# 🚀 Infrastructure Deployment Workflow

The Terraform deployment performs the following actions:

1. Creates an Azure Resource Group.
2. Creates a Virtual Network.
3. Creates a Public Subnet inside the Virtual Network.
4. Creates an Azure Standard Load Balancer.
5. Associates the Load Balancer with a Public IP address.

---

# 📁 Terraform Configuration

## File Structure

Recommended project structure:

```text
azure-infra/
│
├── main.tf
├── variables.tf
├── outputs.tf
└── providers.tf
```

---

# ⚙️ Terraform Main Configuration

## File: `azure-infra/main.tf`

```hcl id="g2b6tq"
resource "azurerm_resource_group" "main" {

  name     = "devops-infra-rg"
  location = "South India"

}


resource "azurerm_virtual_network" "main" {

  name                = "devops-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name

}


resource "azurerm_subnet" "public" {

  name                 = "public-subnet"
  resource_group_name  = azurerm_resource_group.main.name
  virtual_network_name = azurerm_virtual_network.main.name
  address_prefixes     = ["10.0.1.0/24"]

}


resource "azurerm_lb" "main" {

  name                = "devops-lb"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  sku                 = "Standard"

  frontend_ip_configuration {

    name = "PublicIPAddress"

    public_ip_address_id = azurerm_public_ip.lb.id

  }

}
```

---

# 🔍 Resource Configuration Details

## 1. Azure Resource Group

### Resource Name

```text
devops-infra-rg
```

### Purpose

Acts as a logical container for Azure infrastructure resources.

Configuration:

| Setting  | Value             |
| -------- | ----------------- |
| Name     | `devops-infra-rg` |
| Location | `South India`     |

---

## 2. Azure Virtual Network (VNet)

### Resource Name

```text
devops-vnet
```

### Purpose

Provides an isolated network environment for Azure resources.

Configuration:

| Setting        | Value             |
| -------------- | ----------------- |
| Address Space  | `10.0.0.0/16`     |
| Resource Group | `devops-infra-rg` |
| Location       | South India       |

---

## 3. Azure Subnet

### Resource Name

```text
public-subnet
```

### Purpose

Creates a dedicated network segment inside the Virtual Network.

Configuration:

| Setting         | Value         |
| --------------- | ------------- |
| Address Prefix  | `10.0.1.0/24` |
| Virtual Network | `devops-vnet` |

---

## 4. Azure Load Balancer

### Resource Name

```text
devops-lb
```

### Purpose

Provides traffic distribution and high availability for backend services.

Configuration:

| Setting                | Value                     |
| ---------------------- | ------------------------- |
| SKU                    | Standard                  |
| Frontend Configuration | PublicIPAddress           |
| Public IP Reference    | `azurerm_public_ip.lb.id` |

---

# 🔄 Terraform Deployment Commands

## Initialize Terraform

```bash
terraform init
```

Initializes the Terraform working directory and downloads required providers.

---

## Validate Configuration

```bash
terraform validate
```

Checks Terraform syntax and configuration validity.

---

## Review Infrastructure Changes

```bash
terraform plan
```

Displays the resources that Terraform will create, modify, or remove.

> [!IMPORTANT]
> Always review the Terraform plan before applying infrastructure changes, especially in production environments.

---

## Deploy Infrastructure

```bash
terraform apply
```

Creates the Azure resources defined in the Terraform configuration.

---

# 🔐 Infrastructure as Code Best Practices

## Use Version Control

Store Terraform configuration files in a source control repository.

Benefits:

* Tracks infrastructure changes.
* Enables collaboration.
* Provides change history.

---

## Use Remote Terraform State

Store Terraform state securely using Azure Storage.

Benefits:

* Enables team collaboration.
* Prevents local state conflicts.
* Supports state locking.

---

## Separate Environments

Maintain independent configurations for different environments.

Example:

```text
environments/

├── dev/
├── staging/
└── prod/
```

Benefits:

* Reduces accidental production changes.
* Supports controlled deployments.
* Improves environment management.

---

# ✅ Best Practice Recommendations

## Use Modular Terraform Design

Create reusable Terraform modules for common infrastructure components.

Example:

```text
modules/

├── networking/
├── loadbalancer/
└── security/
```

Benefits:

* Improves maintainability.
* Reduces duplicated code.
* Enables standardized deployments.

---

## Enable Resource Tagging

Apply tags to Azure resources for easier management.

Example:

```hcl
tags = {

  Environment = "Production"
  Project     = "DevOps"

}
```

Benefits:

* Simplifies cost tracking.
* Improves resource organization.
* Supports governance requirements.

---

# 📚 Summary

This Terraform-based Azure infrastructure project automates the deployment of core networking components:

* 🏢 Azure Resource Group.
* 🌐 Virtual Network.
* 🔗 Public Subnet.
* ⚖️ Standard Load Balancer.

Using Terraform provides:

* Consistent infrastructure deployment.
* Version-controlled infrastructure changes.
* Repeatable cloud environments.
* Improved operational efficiency.

This approach supports modern DevOps practices and scalable Azure infrastructure management.
