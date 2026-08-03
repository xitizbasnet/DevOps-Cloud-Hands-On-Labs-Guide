# Lab 12 — Install and Configure Jenkins on Amazon Linux 2

> [!IMPORTANT]
> This lab demonstrates how to install Jenkins on an **Amazon Linux 2 EC2 instance**, access the Jenkins Web UI, unlock the installation, and configure essential plugins.

## Objective

Install Jenkins on Amazon Linux 2, access the Web UI, unlock Jenkins, and configure the initial setup.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ An AWS account.
* ✅ Access to the AWS Management Console.
* ✅ Basic EC2 and SSH knowledge.
* ✅ A browser for accessing the Jenkins Web UI.

---

# Procedure

## Step 1 — Launch an Amazon Linux 2 EC2 Instance

Create a new EC2 instance with the following configuration:

| Setting                  | Value                    |
| ------------------------ | ------------------------ |
| **AMI**                  | Amazon Linux 2           |
| **Instance Type**        | `t2.micro`               |
| **Security Group Ports** | SSH (22), Jenkins (8080) |

Configure Security Group inbound rules:

| Protocol | Port | Purpose                      |
| -------- | ---- | ---------------------------- |
| SSH      | 22   | Remote server administration |
| TCP      | 8080 | Jenkins Web UI access        |

> [!WARNING]
> Port 8080 is opened for lab access. In production environments, Jenkins should not be exposed directly to the public internet.

---

## Step 2 — Connect to the EC2 Instance

SSH into the Amazon Linux 2 instance.

Example:

```bash id="v9q5cx"
ssh -i <key.pem> ec2-user@<EC2-PUBLIC-IP>
```

---

## Step 3 — Install Java and Jenkins

### Update System Packages

```bash id="1f6g8n"
sudo yum update -y
```

---

### Install Java OpenJDK 11

```bash id="4t2m8q"
sudo amazon-linux-extras install java-openjdk11 -y
```

---

### Configure Jenkins Repository

Download the Jenkins repository configuration:

```bash id="7h5p3k"
sudo wget -O /etc/yum.repos.d/jenkins.repo \
https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

Import the Jenkins repository key:

```bash id="3j8n6v"
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

---

### Install Jenkins

Install Jenkins:

```bash id="8m4r2w"
sudo yum install jenkins -y
```

---

### Start and Enable Jenkins Service

Start Jenkins:

```bash id="6p9s4t"
sudo systemctl start jenkins
```

Enable Jenkins at system startup:

```bash id="2n7q5m"
sudo systemctl enable jenkins
```

Check Jenkins status:

```bash id="0k5x8v"
sudo systemctl status jenkins
```

---

## Step 4 — Retrieve the Initial Jenkins Administrator Password

Run:

```bash id="5v9m2q"
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the displayed password.

---

## Step 5 — Access Jenkins Web UI

Open a browser and navigate to:

```text id="8w3k5m"
http://<EC2-PUBLIC-IP>:8080
```

Replace:

```text id="3q7h9n"
<EC2-PUBLIC-IP>
```

with the public IP address of your Jenkins EC2 instance.

---

## Step 6 — Complete Jenkins Initial Setup

1. Paste the initial administrator password.

2. Select:

   ```text
   Install Suggested Plugins
   ```

3. Create an administrator user.

---

## Step 7 — Install Additional Jenkins Plugins

Navigate to:

```text id="7c4m8p"
Manage Jenkins → Manage Plugins → Available
```

Install the following plugins:

| Plugin     | Purpose                               |
| ---------- | ------------------------------------- |
| Git        | Source code integration               |
| Pipeline   | CI/CD workflow automation             |
| Blue Ocean | Modern Jenkins pipeline visualization |

---

# Jenkins Installation Workflow

The setup process follows this flow:

```text id="4v9x2k"
EC2 Instance
      |
      ↓
Install Java
      |
      ↓
Install Jenkins
      |
      ↓
Start Jenkins Service
      |
      ↓
Access Web UI :8080
      |
      ↓
Install Plugins
      |
      ↓
Create Admin User
```

---

# Best Practice Tips

> [!TIP]
> Apply these recommendations when using Jenkins in production environments.

## 🔒 Use Reverse Proxy with SSL

In production:

* Run Jenkins behind **Nginx reverse proxy**.
* Enable HTTPS with SSL certificates.
* Avoid exposing port `8080` directly to the internet.

---

## 👤 Use a Dedicated Jenkins User

Configure Jenkins to run with the dedicated:

```text id="8k2p6x"
jenkins
```

OS user.

Avoid:

* Running Jenkins as root.
* Granting unnecessary system privileges.

---

## 💾 Configure Jenkins Backups

Set up regular Jenkins backups.

Recommended option:

```text id="2r7m9v"
ThinBackup Plugin
```

Backup important Jenkins data:

* Job configurations.
* Credentials.
* Pipeline definitions.
* Plugin settings.

---

## 🖥️ Allocate Appropriate Resources

For stable Jenkins operation:

* Minimum recommended RAM:

```text id="9q4m7x"
2 GB RAM
```

Recommended upgrade:

```text id="5k8p3n"
t3.small
```

for better performance.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Amazon Linux 2 EC2 instance created.
* ✅ Security Group configured for SSH and Jenkins access.
* ✅ Java installed successfully.
* ✅ Jenkins installed successfully.
* ✅ Jenkins service started and enabled.
* ✅ Initial administrator password retrieved.
* ✅ Jenkins Web UI accessible.
* ✅ Admin user created.
* ✅ Required plugins installed.

---

# Summary

In this lab, you:

* Created an Amazon Linux 2 EC2 instance.
* Installed Java and Jenkins.
* Started and configured the Jenkins service.
* Accessed Jenkins through the Web UI.
* Installed essential CI/CD plugins.
* Learned Jenkins security and resource management best practices.

You are now ready to continue with the next lab in the **Git & GitHub | CI/CD With Jenkins** learning path.
