# Lab 11 — GitHub Setup

> [!IMPORTANT]
> This lab explains how to connect your local Git repository with GitHub using **SSH key authentication** and push your first project to a remote repository.

## Objective

Create a GitHub account, configure SSH key authentication, and push a local repository to a remote GitHub repository.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 10 — Git Installation & Setup**.
* ✅ A GitHub account.
* ✅ Git installed and configured on your Ubuntu EC2 instance.
* ✅ An existing local repository (`~/my-devops-repo`).

---

# Procedure

## Step 1 — Create a GitHub Account

1. Navigate to:

   ```text
   https://github.com
   ```

2. Create a GitHub account.

3. Use a **professional email address** for your account.

> [!TIP]
> A professional GitHub account profile is useful for collaboration, open-source contributions, and showcasing DevOps projects.

---

## Step 2 — Generate an SSH Key on EC2

Generate an ED25519 SSH key pair:

```bash id="0g9f4x"
ssh-keygen -t ed25519 -C "vinod@example.com"
```

Display the public key:

```bash id="4m8j1s"
cat ~/.ssh/id_ed25519.pub
```

Copy the displayed public key.

---

## Step 3 — Add SSH Key to GitHub

In GitHub:

1. Navigate to:

   ```text
   Settings → SSH and GPG Keys
   ```

2. Select:

   ```text
   New SSH Key
   ```

3. Paste the copied public key.

4. Save the SSH key.

---

## Step 4 — Test GitHub SSH Connection

Test the SSH authentication:

```bash id="8k3m5v"
ssh -T git@github.com
```

Expected successful message:

```text id="f2n9kp"
Hi YOUR_USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

> [!NOTE]
> The username in the response will match your GitHub account.

---

## Step 5 — Create a GitHub Repository

Create a new repository on GitHub:

Repository settings:

| Setting                   | Value                   |
| ------------------------- | ----------------------- |
| **Repository Name**       | `DevOps-Labs`           |
| **Visibility**            | Public                  |
| **Initialize Repository** | No initialization files |

Do not create:

* README file.
* `.gitignore`.
* License.

The existing local repository will be pushed instead.

---

## Step 6 — Push Local Repository to GitHub

Navigate to your local repository:

```bash id="5x2r9n"
cd ~/my-devops-repo
```

Add the GitHub remote repository:

```bash id="6n4p8w"
git remote add origin git@github.com:YOUR_USERNAME/DevOps-Labs.git
```

Rename the default branch to `main`:

```bash id="9r3m7q"
git branch -M main
```

Push the repository:

```bash id="2k8v5h"
git push -u origin main
```

Replace:

```text id="j4s9pw"
YOUR_USERNAME
```

with your actual GitHub username.

---

# GitHub SSH Workflow Overview

The workflow follows this process:

```text
Local Git Repository
        |
        | SSH Authentication
        ↓
GitHub Remote Repository
        |
        | git push
        ↓
Remote Code Repository
```

---

# Best Practice Tips

> [!TIP]
> Use secure authentication methods and maintain a professional GitHub presence for DevOps collaboration.

## 🔐 Use SSH Keys Instead of HTTPS Passwords

Use SSH authentication because it provides:

* More secure authentication.
* No password entry for every operation.
* No token expiration management.

Avoid storing passwords or personal access tokens unnecessarily.

---

## 🔒 Enable Two-Factor Authentication (2FA)

Enable **2FA on GitHub**.

Benefits:

* Protects your account from unauthorized access.
* Required by many organizations for repository access.

---

## 👤 Create a Professional GitHub Profile README

Create a GitHub profile README to showcase:

* Technical skills.
* DevOps projects.
* Certifications.
* Open-source contributions.
* Learning progress.

Recruiters and engineering teams often review GitHub profiles.

---

## ⭐ Explore Popular DevOps Repositories

Learn from the community by:

* ⭐ Starring popular DevOps projects.
* 🍴 Forking repositories.
* 📖 Reviewing real-world implementation patterns.

Examples:

* Kubernetes
* Ansible
* Terraform

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ GitHub account created.
* ✅ SSH key generated on EC2.
* ✅ SSH public key added to GitHub.
* ✅ GitHub SSH connection tested successfully.
* ✅ Remote GitHub repository created.
* ✅ Local repository pushed successfully.
* ✅ Code is visible in the GitHub repository.

---

# Summary

In this lab, you:

* Created and configured a GitHub account.
* Generated an SSH key for secure authentication.
* Connected your EC2 Git environment with GitHub.
* Created a remote repository.
* Pushed a local Git project to GitHub.
* Learned GitHub security and collaboration best practices.

You are now ready to continue with the next lab in the **Git & GitHub | CI/CD With Jenkins** learning path.
