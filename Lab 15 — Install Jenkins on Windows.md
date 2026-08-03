# Lab 15 — Install Jenkins on Windows

> [!IMPORTANT]
> This lab demonstrates how to install and configure Jenkins on **Windows** for local development and testing scenarios.

## Objective

Install Jenkins on Windows for local development / testing scenarios.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ A Windows operating system.
* ✅ Administrator access on the machine.
* ✅ Internet access for downloading required software.
* ✅ Basic understanding of Jenkins and Java requirements.

---

# Procedure

## Step 1 — Install Java JDK 11

Download Java JDK 11 from:

```text
https://adoptium.net
```

Install Java and configure the environment variable:

```text
JAVA_HOME
```

Verify Java installation:

Open PowerShell:

```powershell id="8v4m2q"
java -version
```

Example output:

```text
java version "11.x.x"
```

---

## Step 2 — Download and Install Jenkins LTS

Download Jenkins LTS from:

```text
https://www.jenkins.io/download/
```

Steps:

1. Download the **Jenkins LTS MSI installer**.
2. Run the installer.
3. Follow the installation wizard.

---

## Step 3 — Verify Jenkins Windows Service

During installation:

* Jenkins is installed as a **Windows Service**.
* Jenkins runs by default on:

```text
Port: 8080
```

Verify the Jenkins service:

```powershell id="1p7x9n"
net start Jenkins
```

To stop Jenkins:

```powershell id="6k3m8v"
net stop Jenkins
```

---

## Step 4 — Access Jenkins Web UI

Open a browser and navigate to:

```text id="9q2m5x"
http://localhost:8080
```

Complete the Jenkins setup wizard:

1. Unlock Jenkins.
2. Install suggested plugins.
3. Create the administrator user.
4. Complete the initial configuration.

---

## Step 5 — Configure Java in Jenkins

Navigate to:

```text id="4r8m1k"
Manage Jenkins → Configure System
```

Configure the Java installation:

```text
JAVA_HOME
```

Set the path to your installed Java JDK 11 location.

Example:

```text
C:\Program Files\Eclipse Adoptium\jdk-11
```

---

## Step 6 — Install Required Jenkins Plugins

Navigate to:

```text id="7n3m8p"
Manage Jenkins → Manage Plugins
```

Install:

| Plugin     | Purpose                    |
| ---------- | -------------------------- |
| Git        | Source code management     |
| Pipeline   | CI/CD automation workflows |
| Blue Ocean | Pipeline visualization     |

---

# Jenkins Windows Verification

## Verify Java Installation

Open PowerShell:

```powershell id="5m9v2x"
java -version
```

---

## Control Jenkins Service

Start Jenkins:

```powershell id="8h4k6q"
net start Jenkins
```

Stop Jenkins:

```powershell id="2v7n5m"
net stop Jenkins
```

---

# Jenkins Windows Workflow

The local Windows Jenkins workflow:

```text id="6x9m3p"
Windows Machine
        |
        ↓
Install Java JDK 11
        |
        ↓
Install Jenkins MSI
        |
        ↓
Run Jenkins Service
        |
        ↓
Access localhost:8080
        |
        ↓
Install Plugins
        |
        ↓
Create CI/CD Jobs
```

---

# Best Practice Tips

> [!TIP]
> Windows Jenkins is useful for learning and local testing. For enterprise environments, use a dedicated Linux-based Jenkins server.

## 🖥️ Use Linux Jenkins for Production

Windows Jenkins is suitable for:

* Development.
* Testing.
* Personal learning environments.

For team and production environments:

* Use Linux-based Jenkins servers.
* Deploy Jenkins on dedicated infrastructure.

---

## 🛡️ Configure Windows Defender Exclusions

Add Jenkins directories to Windows Defender exclusions.

Benefits:

* Reduces unnecessary scanning overhead.
* Improves build performance.
* Prevents workspace-related slowdowns.

---

## 💾 Configure Jenkins Data Directory

Store Jenkins data on a dedicated drive:

Recommended:

```text
D:
```

instead of:

```text
C:
```

Benefits:

* Better disk space management.
* Prevents system drive exhaustion.
* Simplifies backup management.

---

## ⚙️ Configure Jenkins JVM Memory

Edit the Jenkins configuration file:

```text
jenkins.xml
```

Add JVM memory settings:

```text
-Xmx1024m
```

This increases the maximum Java heap memory available to Jenkins.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Java JDK 11 installed.
* ✅ `JAVA_HOME` configured.
* ✅ Jenkins LTS installed through MSI.
* ✅ Jenkins Windows service running.
* ✅ Jenkins Web UI accessible at `http://localhost:8080`.
* ✅ Required plugins installed.
* ✅ Jenkins Java configuration completed.

---

# Summary

In this lab, you:

* Installed Java JDK 11 on Windows.
* Installed Jenkins LTS using the MSI installer.
* Configured Jenkins as a Windows service.
* Accessed the Jenkins Web UI.
* Installed essential CI/CD plugins.
* Learned Windows-specific Jenkins administration practices.

You are now ready to continue with the next lab in the **Git & GitHub | CI/CD With Jenkins** learning path.
