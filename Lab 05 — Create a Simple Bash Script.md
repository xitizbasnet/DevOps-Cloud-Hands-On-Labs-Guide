# Section 2 — Linux & Shell/Bash Scripting

## Lab 05 — Create a Simple Bash Script

> [!IMPORTANT]
> This lab introduces the fundamentals of Bash scripting by creating, making executable, and running a simple shell script on your Amazon EC2 instance.

## Objective

Write, make executable, and run a basic shell script on your EC2 instance.

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 04 — Create & Connect to Ubuntu EC2 Instance on AWS**.
* ✅ Successfully connected to your EC2 instance using SSH.
* ✅ Basic familiarity with Linux terminal commands.

---

## Procedure

### Step 1 — Connect to Your EC2 Instance and Create a Scripts Directory

SSH into your EC2 instance (Lab 04).

Create a directory for storing your scripts and navigate to it:

```bash
mkdir -p ~/scripts && cd ~/scripts
```

---

### Step 2 — Create Your First Bash Script

Create a new script named **`hello_devops.sh`**:

```bash
nano hello_devops.sh
```

Add the following content:

```bash
#!/bin/bash

# DevOps Hello World Script

echo "============================"
echo " Welcome to DevOps Lab!"
echo " Hostname: $(hostname)"
echo " Date: $(date)"
echo " User: $(whoami)"
echo "============================"
```

---

### Step 3 — Save, Make the Script Executable, and Run It

Save the file:

* Press **Ctrl + O**
* Press **Enter**
* Press **Ctrl + X**

Make the script executable and execute it:

```bash
chmod +x hello_devops.sh
./hello_devops.sh
```

---

## Expected Output

After running the script, you should see output similar to the following:

```text
============================
 Welcome to DevOps Lab!
 Hostname: <hostname>
 Date: <current date and time>
 User: <current user>
============================
```

---

## Best Practice Tips

> [!TIP]
> Following Bash scripting best practices improves script reliability, readability, and maintainability.

### 📝 Always Use a Shebang

Begin every Bash script with:

```bash
#!/bin/bash
```

This specifies the interpreter that should execute the script.

---

### 🛡️ Enable Safe Bash Options

Add the following line near the beginning of your scripts:

```bash
set -euo pipefail
```

This helps by:

* **`-e`** — Exiting immediately if a command fails.
* **`-u`** — Treating unset variables as errors.
* **`-o pipefail`** — Detecting failures within command pipelines.

---

### 💬 Comment Your Scripts

Use comments to explain:

* The purpose of the script.
* Important logic.
* Configuration values.
* Complex commands.

Well-documented scripts are easier to maintain for both yourself and your teammates.

---

### ✅ Validate Scripts with ShellCheck

Install **ShellCheck** to identify common Bash scripting issues before using scripts in production.

Install ShellCheck:

```bash
sudo apt install shellcheck
```

Analyze your script:

```bash
shellcheck hello_devops.sh
```

---

## Validation

Verify that the following tasks have been completed successfully:

* ✅ Connected to the EC2 instance.
* ✅ Created the `~/scripts` directory.
* ✅ Created the `hello_devops.sh` script.
* ✅ Added the provided script content.
* ✅ Made the script executable.
* ✅ Successfully executed the script.
* ✅ Displayed the hostname, current date, and logged-in user.

---

## Summary

In this lab, you:

* Created your first Bash script.
* Learned the purpose of the **shebang (`#!/bin/bash`)**.
* Made a script executable using `chmod`.
* Executed a Bash script from the Linux terminal.
* Learned foundational Bash scripting best practices, including safe execution options, commenting, and script validation with ShellCheck.

You are now ready to continue with the next lab in the **Linux & Shell/Bash Scripting** learning path.
