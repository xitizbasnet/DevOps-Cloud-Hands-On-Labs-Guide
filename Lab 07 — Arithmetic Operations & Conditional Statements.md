# Lab 07 — Arithmetic Operations & Conditional Statements

> [!IMPORTANT]
> This lab introduces Bash arithmetic operations and conditional statements. You will perform mathematical calculations, evaluate numeric and string conditions, and check for the existence of files.

## Objective

Perform math and write **if**, **elif**, and **else** logic in Bash.

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 06 — Define Variables in a Bash Script**.
* ✅ Access to your EC2 instance via SSH.
* ✅ Basic understanding of Bash variables.

---

## Procedure

### Step 1 — Create the Arithmetic and Conditional Script

Create a new script (for example, `arithmetic_conditions.sh`) and add the following content:

```bash
#!/bin/bash

# Arithmetic
A=20
B=5

echo "Add: $((A+B)) | Sub: $((A-B)) | Mul: $((A*B)) | Div: $((A/B)) | Mod: $((A%B))"

# Conditionals
CPU_USAGE=85

if [ $CPU_USAGE -gt 90 ]; then
    echo "CRITICAL: CPU at ${CPU_USAGE}%"
elif [ $CPU_USAGE -gt 75 ]; then
    echo "WARNING: CPU at ${CPU_USAGE}%"
else
    echo "OK: CPU at ${CPU_USAGE}%"
fi

# String conditional
ENV="production"

if [[ "$ENV" == "production" ]]; then
    echo "Deploy with approval required!"
fi

# File check
LOG="/var/log/syslog"

if [ -f "$LOG" ]; then
    echo "Log file exists: $LOG"
fi
```

---

### Step 2 — Make the Script Executable

Run:

```bash
chmod +x arithmetic_conditions.sh
```

---

### Step 3 — Execute the Script

Run:

```bash
./arithmetic_conditions.sh
```

Observe the output produced by each section of the script.

---

## Understanding the Script

### ➕ Arithmetic Operations

The script performs the following calculations using Bash arithmetic expansion:

| Operation      | Expression |
| -------------- | ---------- |
| Addition       | `$((A+B))` |
| Subtraction    | `$((A-B))` |
| Multiplication | `$((A*B))` |
| Division       | `$((A/B))` |
| Modulus        | `$((A%B))` |

---

### ⚖️ Numeric Conditional

The script checks CPU utilization and displays a status message:

| Condition | Output       |
| --------- | ------------ |
| CPU > 90  | **CRITICAL** |
| CPU > 75  | **WARNING**  |
| Otherwise | **OK**       |

---

### 🔤 String Comparison

The script evaluates whether the deployment environment is:

```text
production
```

If true, it displays:

```text
Deploy with approval required!
```

---

### 📄 File Existence Check

The script verifies whether the following log file exists:

```text
/var/log/syslog
```

If present, it prints the file location.

---

## Expected Output

Example output:

```text
Add: 25 | Sub: 15 | Mul: 100 | Div: 4 | Mod: 0
WARNING: CPU at 85%
Deploy with approval required!
Log file exists: /var/log/syslog
```

> [!NOTE]
> The final line may vary depending on the Linux distribution. Some systems use `/var/log/messages` instead of `/var/log/syslog`.

---

## Best Practice Tips

> [!TIP]
> Following these Bash scripting practices results in safer, more maintainable automation scripts.

### 🧮 Use the Appropriate Test Syntax

Use:

* `(( ))` for arithmetic operations.
* `[[ ]]` for string comparisons and advanced conditional expressions.

These are generally more robust than the traditional `[ ]` syntax.

---

### 📝 Always Quote Variables

Always reference variables using quotes:

```bash
"$VAR"
```

This prevents:

* Word splitting
* Filename expansion (globbing)
* Unexpected script behavior

---

### 🔢 Use the Correct Comparison Operators

For numeric comparisons:

| Operator | Meaning      |
| -------- | ------------ |
| `-gt`    | Greater than |
| `-lt`    | Less than    |
| `-eq`    | Equal to     |

For string comparisons inside `[[ ]]`:

| Operator | Meaning   |
| -------- | --------- |
| `==`     | Equal     |
| `!=`     | Not equal |

---

### 🚀 Add Health Checks to Automation

Implement conditional health checks in deployment scripts to:

* Validate application health.
* Detect deployment failures.
* Trigger automated rollback procedures when necessary.

This is a common DevOps practice for improving deployment reliability.

---

## Validation

Verify that the following tasks have been completed successfully:

* ✅ Arithmetic calculations executed correctly.
* ✅ Numeric conditions evaluated as expected.
* ✅ String comparison completed successfully.
* ✅ File existence check performed.
* ✅ Script executed without errors.

---

## Summary

In this lab, you:

* Performed arithmetic operations using Bash.
* Implemented `if`, `elif`, and `else` conditional statements.
* Compared numeric and string values.
* Checked for file existence.
* Learned best practices for writing reliable Bash conditional logic.

You are now ready to continue with the next lab in the **Linux & Shell/Bash Scripting** learning path.
