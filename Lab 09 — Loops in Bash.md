# Lab 09 — Loops in Bash

> [!IMPORTANT]
> This lab introduces Bash looping constructs used for automation tasks, including **for loops**, **while loops**, and **until-style conditions**. These patterns are commonly used in DevOps scripts for server checks, retries, monitoring, and batch processing.

## Objective

Use the following Bash loops for automation tasks:

* 🔁 `for` loops
* 🔁 `while` loops
* 🔁 `until` loops

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 08 — Comparison & Logical Operators**.
* ✅ Access to a Linux/EC2 instance.
* ✅ Basic understanding of Bash variables and conditional statements.

---

# Procedure

## Step 1 — Create the Bash Loops Script

Create a new script:

```bash id="6u7p6a"
nano loops_demo.sh
```

Add the following content:

```bash id="3y3j7q"
#!/bin/bash

# FOR loop — iterate list

SERVERS=("web-01" "web-02" "app-01" "db-01")

for server in "${SERVERS[@]}"; do
    echo "Checking $server..."

    # ping -c1 $server &>/dev/null && echo " UP" || echo " DOWN"

done


# FOR loop — range (retry logic)

for i in $(seq 1 5); do
    echo "Attempt $i of 5..."
    sleep 1
done


# WHILE loop — wait for service

COUNTER=0

while ! systemctl is-active --quiet nginx 2>/dev/null; do
    echo "Waiting for nginx... ($COUNTER)"
    sleep 2

    ((COUNTER++))

    [ $COUNTER -ge 10 ] && {
        echo "Timeout!"
        exit 1
    }

done

echo "Nginx is running."


# Process log files

while IFS= read -r line; do
    echo "LOG: $line"
done < /var/log/syslog | head -5
```

---

## Step 2 — Make the Script Executable

Run:

```bash id="s5m7px"
chmod +x loops_demo.sh
```

---

## Step 3 — Validate Script Syntax

Before execution, validate the script:

```bash id="7k5x5p"
bash -n loops_demo.sh
```

---

## Step 4 — Execute the Script

Run:

```bash id="5jv4j3"
./loops_demo.sh
```

Observe how each loop performs its automation task.

---

# Understanding the Script

## 🔁 FOR Loop — Iterate Through a List

The script defines a list of servers:

```bash
SERVERS=("web-01" "web-02" "app-01" "db-01")
```

The loop processes each server:

```bash
for server in "${SERVERS[@]}"; do
```

Example use cases:

* Server health checks.
* Deployment operations.
* Configuration updates.

---

## 🔢 FOR Loop — Range-Based Retry Logic

The script performs five attempts:

```bash
for i in $(seq 1 5)
```

Example output:

```text
Attempt 1 of 5...
Attempt 2 of 5...
Attempt 3 of 5...
```

Common DevOps uses:

* Retry failed deployments.
* Wait for resources.
* Poll external systems.

---

## ⏳ WHILE Loop — Service Availability Check

The script waits until Nginx becomes active:

```bash
while ! systemctl is-active --quiet nginx
```

The loop:

1. Checks the service status.
2. Waits for two seconds.
3. Increases the retry counter.
4. Stops after 10 attempts.

If the timeout is reached:

```text
Timeout!
```

is displayed and the script exits.

---

## 📄 WHILE Loop — Process Log Files

The script reads log file contents line by line:

```bash
while IFS= read -r line
```

It processes:

```text
/var/log/syslog
```

and displays the first five lines.

---

# Expected Output

Example output:

```text
Checking web-01...
Checking web-02...
Checking app-01...
Checking db-01...
Attempt 1 of 5...
Attempt 2 of 5...
Attempt 3 of 5...
Waiting for nginx... (0)
Nginx is running.
LOG: <log entry>
```

> [!NOTE]
> Output may vary depending on your server configuration and whether the Nginx service is installed and running.

---

# Best Practice Tips

> [!TIP]
> Loops are widely used in DevOps automation. Use them carefully to avoid unexpected behavior or resource issues.

## 📁 Handle Empty File Matches Safely

When using:

```bash
for f in *.txt
```

be careful with empty matches.

Enable `nullglob` when required:

```bash
shopt -s nullglob
```

This prevents the loop from processing invalid filenames.

---

## ⏱️ Add Timeout Logic

Always add timeout handling in `while` loops.

This prevents:

* Infinite loops.
* Hanging automation pipelines.
* Stuck deployment processes.

Example:

```bash
if [ $COUNTER -ge 10 ]; then
    exit 1
fi
```

---

## ⏭️ Control Loop Flow

Use:

### `break`

Stops the loop immediately.

Example:

```bash
break
```

### `continue`

Skips the current iteration and moves to the next one.

Example:

```bash
continue
```

---

## ⚡ Parallelize Loops for Performance

For independent tasks, run loops in parallel:

```bash
for s in ${SERVERS[@]}; do
    check $s &
done

wait
```

Benefits:

* Faster execution.
* Better resource utilization.
* Useful for large server fleets.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Created a Bash loop script.
* ✅ Used a `for` loop with arrays.
* ✅ Implemented retry logic using a range loop.
* ✅ Created a `while` loop for service monitoring.
* ✅ Processed log files using a loop.
* ✅ Added timeout handling.
* ✅ Validated and executed the script successfully.

---

# Summary

In this lab, you:

* Used Bash `for` loops for repeated automation tasks.
* Implemented retry logic.
* Created service monitoring using `while` loops.
* Processed log files automatically.
* Learned best practices for safe and efficient loop usage in DevOps scripts.

You are now ready to continue with the next lab in the **Linux & Shell/Bash Scripting** learning path.
