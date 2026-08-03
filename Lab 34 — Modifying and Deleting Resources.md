# Lab 34 — Modifying and Deleting Resources

> [!IMPORTANT]
> This lab explains how Terraform manages infrastructure changes, including resource updates, replacements, state management, importing existing resources, and controlled deletion.

## Objective

Understand:

* 📋 `terraform plan` output.
* 🔄 In-place updates vs destroy-and-recreate changes.
* 📦 Terraform state management.
* 🎯 Targeted resource operations.
* 🗑️ Resource deletion workflows.

By the end of this lab, you will understand how Terraform detects and applies infrastructure changes safely.

---

# Prerequisites

Before starting, ensure:

* ✅ Terraform installed and configured.
* ✅ Previous AWS Terraform resources created.
* ✅ Access to Terraform project files.
* ✅ AWS credentials configured.

---

# Terraform Resource Change Workflow

Terraform compares:

```text
Current Infrastructure State
          |
          ↓
Terraform State File
          |
          ↓
Terraform Configuration (.tf)
          |
          ↓
terraform plan
          |
          ↓
Apply Changes
```

---

# Part 1 — Modify Existing Resources

## Step 1 — Change EC2 Instance Type

Open:

```text
ec2.tf
```

Modify:

```hcl
instance_type = "t2.micro"
```

Change to:

```hcl
instance_type = "t3.micro"
```

---

## Step 2 — Review Terraform Plan

Run:

```bash
terraform plan
```

Terraform displays the required changes.

Example:

```text
~ update in-place

-/+ destroy and recreate
```

---

# Understanding Terraform Plan Symbols

| Symbol | Meaning                          |
| ------ | -------------------------------- |
| `+`    | Resource will be created         |
| `-`    | Resource will be destroyed       |
| `~`    | Resource updated in-place        |
| `-/+`  | Resource destroyed and recreated |

---

# In-Place Update vs Replacement

## In-Place Update

Example:

```text
~ update
```

Characteristics:

* Resource remains the same.
* Usually no downtime.
* Terraform modifies existing configuration.

---

## Destroy and Recreate

Example:

```text
-/+
```

Characteristics:

* Existing resource deleted.
* New resource created.
* May cause downtime.

---

# Part 2 — Apply Changes

## Step 3 — Apply Specific Change

Run:

```bash
terraform apply
```

Terraform will:

* Review planned changes.
* Ask for confirmation.
* Update infrastructure.

---

# Part 3 — Inspect Terraform State

Terraform state tracks managed resources.

## Step 4 — View Current State

Run:

```bash
terraform show
```

Displays:

* Current infrastructure details.
* Resource attributes.
* Terraform-managed values.

---

## Step 5 — List Resources in State

Run:

```bash
terraform state list
```

Example:

```text
aws_instance.web
aws_security_group.web_sg
aws_key_pair.devops_key
```

---

## Step 6 — View Specific Resource State

Run:

```bash
terraform state show aws_instance.web
```

Displays:

* Instance details.
* Attributes.
* Terraform state information.

---

# Part 4 — Remove Resource From Terraform State

## Step 7 — Remove Resource Without Destroying

Run:

```bash
terraform state rm aws_instance.web
```

Effect:

* Removes resource from Terraform tracking.
* Does not delete the actual AWS resource.

Use cases:

* Migrating resources.
* Changing Terraform ownership.
* Removing accidental management.

---

# Part 5 — Import Existing Resources

## Step 8 — Import AWS Resource

Command:

```bash
terraform import aws_instance.web i-0abc123def456
```

Purpose:

* Adds existing infrastructure into Terraform state.
* Allows Terraform management of manually created resources.

Example:

```text
Existing AWS EC2
        |
        ↓
terraform import
        |
        ↓
Terraform State
```

---

# Part 6 — Targeted Resource Operations

## Step 9 — Destroy Specific Resource

Run:

```bash
terraform destroy -target=aws_security_group.web_sg
```

Purpose:

* Deletes only the selected resource.
* Leaves other resources untouched.

---

## Step 10 — Destroy Entire Infrastructure

Run:

```bash
terraform destroy
```

Removes:

* All Terraform-managed resources.
* Infrastructure created by this project.

---

# Terraform State Commands Reference

| Command                | Purpose                        |
| ---------------------- | ------------------------------ |
| `terraform show`       | Display current state          |
| `terraform state list` | List managed resources         |
| `terraform state show` | Show resource details          |
| `terraform state rm`   | Remove resource from state     |
| `terraform state mv`   | Rename/move resources in state |
| `terraform import`     | Import existing resources      |

---

# Best Practice Tips

> [!TIP]
> Follow these practices when modifying Terraform-managed infrastructure.

---

## ⚠️ Carefully Review terraform plan

Always check:

```bash
terraform plan
```

before:

```bash
terraform apply
```

Important:

```text
-/+
```

means:

* Destroy existing resource.
* Create replacement resource.
* Possible downtime.

---

## 🛡️ Protect Critical Resources

Use:

```hcl
lifecycle {
  prevent_destroy = true
}
```

Recommended for:

* Databases.
* Production storage.
* Critical infrastructure.

Example:

```hcl
resource "aws_db_instance" "production" {

  lifecycle {

    prevent_destroy = true

  }

}
```

---

## 🔄 Use terraform state mv for Renaming

Instead of recreating resources:

```bash
terraform state mv
```

Use it to:

* Rename resources.
* Refactor configurations.
* Maintain existing infrastructure.

---

## 🚫 Never Edit terraform.tfstate Manually

Avoid:

```text
Manual .tfstate modification
```

Use:

```bash
terraform state
```

commands instead.

Benefits:

* Prevents corruption.
* Maintains state consistency.
* Reduces deployment risks.

---

# Validation Checklist

Verify:

* ✅ Resource changes reviewed using `terraform plan`.
* ✅ Update vs replacement behavior understood.
* ✅ Terraform state inspected.
* ✅ Resources removed safely from state.
* ✅ Existing resources imported successfully.
* ✅ Targeted destroy tested.
* ✅ Full destroy workflow understood.

---

# Summary

In this lab, you learned:

* How Terraform detects infrastructure changes.
* Difference between update and replacement operations.
* How to inspect and manage Terraform state.
* How to import existing resources.
* How to safely remove or destroy resources.

Understanding Terraform state management is essential for reliable Infrastructure as Code operations.
