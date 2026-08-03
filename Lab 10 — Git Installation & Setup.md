# Section 3 — Git & GitHub | CI/CD With Jenkins

## Lab 10 — Git Installation & Setup

> [!IMPORTANT]
> This lab introduces Git installation, configuration, repository creation, and essential Git commands. You will configure Git identity settings and understand the basic three-tree architecture used by Git.

## Objective

Install Git, configure global identity, and understand the **three-tree architecture**.

The Git three-tree architecture consists of:

* 📁 **Working Tree** — Current files and modifications.
* 📦 **Staging Area (Index)** — Changes prepared for commit.
* 🗃️ **Repository** — Permanently stored commits and history.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ An Ubuntu EC2 instance available.
* ✅ SSH access to your EC2 instance.
* ✅ Sudo privileges for installing packages.
* ✅ Basic Linux command-line knowledge.

---

# Procedure

## Step 1 — Install Git on Ubuntu EC2

Update the package repository and install Git:

```bash id="v9w7kk"
sudo apt update && sudo apt install git -y
```

Verify the Git installation:

```bash id="2o9v0a"
git --version
```

Example output:

```text id="7v4p2h"
git version 2.x.x
```

---

## Step 2 — Configure Global Git Identity

Configure your Git username:

```bash id="q4g1ts"
git config --global user.name "Vinod Muleva"
```

Configure your Git email:

```bash id="1s7h3p"
git config --global user.email "vinod@example.com"
```

Configure the default Git editor:

```bash id="j5s1cc"
git config --global core.editor nano
```

Configure the default branch name:

```bash id="m9z4xh"
git config --global init.defaultBranch main
```

View the Git configuration:

```bash id="6w8p2r"
git config --list
```

---

## Step 3 — Create Your First Local Repository

Create a project directory:

```bash id="7y8m9n"
mkdir ~/my-devops-repo
cd ~/my-devops-repo
```

Initialize a Git repository:

```bash id="0c5g3v"
git init
```

Create a README file:

```bash id="8j4m2p"
echo '# DevOps Project' > README.md
```

Add files to the staging area:

```bash id="3g9t6k"
git add .
```

Create the first commit:

```bash id="4s8q1w"
git commit -m "Initial commit: add README"
```

---

# Step 4 — Key Git Commands Reference

The following commands are commonly used during daily Git operations:

| Command             | Description                                  |
| ------------------- | -------------------------------------------- |
| `git status`        | Displays the current working tree status.    |
| `git log --oneline` | Displays commit history in a compact format. |
| `git diff`          | Shows unstaged changes.                      |
| `git diff --staged` | Shows changes that are staged for commit.    |

---

## Git Workflow Overview

The basic Git workflow follows this sequence:

```text
Working Directory
        |
        | git add
        ↓
Staging Area
        |
        | git commit
        ↓
Repository History
```

---

# Best Practice Tips

> [!TIP]
> Following Git best practices improves collaboration, traceability, and maintainability in DevOps environments.

## 📝 Write Clear Commit Messages

Use the **imperative mood** when writing commit messages.

Recommended:

```text
Add feature
```

Avoid:

```text
Added feature
```

Commit messages should describe the action performed.

---

## 🔄 Commit Small and Frequently

Follow these practices:

* Commit often.
* Keep each commit focused on one logical change.
* Avoid combining unrelated modifications in a single commit.

---

## 📄 Add `.gitignore` Immediately

Create a `.gitignore` file at the beginning of your project.

Use:

```text
gitignore.io
```

to generate project-specific `.gitignore` templates.

Common files to ignore:

* Secrets.
* Credentials.
* Build files.
* Temporary files.
* Dependency folders.

---

## ⚡ Create Useful Git Aliases

Create a shortcut for viewing Git history:

```bash id="8u3m4k"
git config --global alias.lg 'log --oneline --graph --all'
```

Use the alias:

```bash id="9v6r2t"
git lg
```

This provides a compact graphical view of repository history.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Git installed successfully.
* ✅ Git version verified.
* ✅ Global username and email configured.
* ✅ Default branch configured as `main`.
* ✅ Local Git repository initialized.
* ✅ README file committed successfully.
* ✅ Basic Git commands tested.

---

# Summary

In this lab, you:

* Installed Git on Ubuntu EC2.
* Configured Git global identity settings.
* Learned the Git three-tree architecture.
* Created your first local repository.
* Created and committed a README file.
* Reviewed essential Git commands and best practices.

You are now ready to continue with the next lab in the **Git & GitHub | CI/CD With Jenkins** learning path.
