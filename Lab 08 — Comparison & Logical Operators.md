# Lab 08 — Comparison & Logical Operators

> [!IMPORTANT]
> This lab introduces Bash comparison and logical operators used in real DevOps automation scenarios, including system health checks, resource monitoring, and validation scripts.

## Objective

Use the following operators in real DevOps scenarios:

* Numeric comparison operators:

  * `-eq`
  * `-ne`
  * `-lt`
  * `-gt`

* Logical operators:

  * `&&`
  * `||`
  * `!`

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 07 — Arithmetic Operations & Conditional Statements**.
* ✅ Access to your Linux/EC2 instance through SSH.
* ✅ Basic understanding of Bash scripting and conditional statements.

---

# Procedure

## Step 1 — Create the Comparison and Logical Operators Script

Create a new script:

```bash id="6qv3qa"
nano comparison_logical.sh
```

Add the following content:

```bash id="6c0x8u"
#!/bin/bash

# Numeric comparisons

FREE_MEM=$(free -m | awk '/^Mem:/{print $4}')

echo "Free memory: ${FREE_MEM}MB"

[ $FREE_MEM -lt 200 ] && echo "LOW MEMORY WARNING" || echo "Memory OK"


# Logical AND: both conditions must be true

DISK=$(df / | awk 'NR==2{print $5}' | tr -d %)

if [[ $DISK -gt 80 && $FREE_MEM -lt 500 ]]; then
    echo "ALERT: System under stress!"
fi


# Logical OR: either condition

if [[ ! -d "/opt/app" || ! -f "/opt/app/config.yml" ]]; then
    echo "App not set up correctly — aborting."
    exit 1
fi


echo "All checks passed."
```

---

## Step 2 — Make the Script Executable

Run:

```bash id="1q6h3y"
chmod +x comparison_logical.sh
```

---

## Step 3 — Validate Script Syntax

Before execution, check the script syntax:

```bash id="9mg5jw"
bash -n comparison_logical.sh
```

A successful syntax check returns no output.

---

## Step 4 — Execute the Script

Run:

```bash id="zq0f6u"
./comparison_logical.sh
```

Observe how the script evaluates memory, disk usage, and application configuration checks.

---

# Understanding the Script

## 🔢 Numeric Comparisons

The script collects available system memory:

```bash
FREE_MEM=$(free -m | awk '/^Mem:/{print $4}')
```

It then checks whether available memory is below the defined threshold:

```bash
[ $FREE_MEM -lt 200 ]
```

If memory is below 200 MB:

```text
LOW MEMORY WARNING
```

Otherwise:

```text
Memory OK
```

---

## 🔗 Logical AND Operator (`&&`)

The script checks whether **both** conditions are true:

```bash
if [[ $DISK -gt 80 && $FREE_MEM -lt 500 ]]; then
```

Conditions:

1. Disk usage is greater than 80%.
2. Free memory is less than 500 MB.

If both conditions match:

```text
ALERT: System under stress!
```

is displayed.

---

## 🔀 Logical OR Operator (`||`)

The script validates application configuration:

```bash
if [[ ! -d "/opt/app" || ! -f "/opt/app/config.yml" ]]; then
```

The condition succeeds if either:

* `/opt/app` directory does not exist.
* `/opt/app/config.yml` file does not exist.

The script then stops execution:

```bash
exit 1
```

---

## ❗ Logical NOT Operator (`!`)

The `!` operator reverses a condition.

Example:

```bash
! -d "/opt/app"
```

Means:

> The `/opt/app` directory does not exist.

---

# Expected Output

Example output when checks pass:

```text
Free memory: 2048MB
Memory OK
All checks passed.
```

Example output when system validation fails:

```text
Free memory: 150MB
LOW MEMORY WARNING
ALERT: System under stress!
```

or:

```text
App not set up correctly — aborting.
```

> [!NOTE]
> Output depends on your system memory, disk usage, and application directory configuration.

---

# Best Practice Tips

> [!TIP]
> Logical operators are heavily used in DevOps automation scripts for validation, monitoring, and deployment workflows.

## ⛓️ Use Command Chains with `&&`

Example:

```bash
command1 && command2
```

Behavior:

* `command2` runs only if `command1` succeeds.

Example:

```bash
mkdir /opt/app && echo "Directory created"
```

---

## 🔄 Use `||` for Fallback Actions

Example:

```bash
command || fallback_command
```

Behavior:

* The fallback command runs if the first command fails.

Example:

```bash
systemctl restart nginx || echo "Restart failed"
```

---

## ♻️ Use Functions for Complex Logic

Wrap complex conditions inside functions to:

* Improve readability.
* Avoid duplicate code.
* Reuse validation logic across multiple scripts.

---

## ✅ Validate Scripts Before Execution

Always check Bash syntax before running scripts:

```bash
bash -n script.sh
```

This helps identify syntax issues before execution.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Used numeric comparison operators.
* ✅ Implemented logical AND (`&&`) conditions.
* ✅ Implemented logical OR (`||`) conditions.
* ✅ Used the logical NOT (`!`) operator.
* ✅ Checked system memory and disk conditions.
* ✅ Validated script syntax before execution.

---

# Summary

In this lab, you:

* Used Bash comparison operators for system checks.
* Applied logical operators in DevOps scenarios.
* Created automated resource validation logic.
* Implemented failure handling using exit codes.
* Learned best practices for writing maintainable Bash automation scripts.

You are now ready to continue with the next lab in the **Linux & Shell/Bash Scripting** learning path.
