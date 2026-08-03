# Lab 13 — Install Git & Maven on Jenkins for Continuous Integration

> [!IMPORTANT]
> This lab demonstrates how to configure **Git** and **Maven** in Jenkins to create a Java-based Continuous Integration (CI) pipeline. You will configure build tools, create a Jenkins Freestyle job, connect it to a GitHub repository, and execute a Maven build.

## Objective

Configure Maven and Git as build tools in Jenkins for a Java CI pipeline.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 12 — Install and Configure Jenkins on Amazon Linux 2**.
* ✅ A running Jenkins server.
* ✅ A GitHub repository containing a Java/Maven project.
* ✅ Jenkins administrator access.

---

# Procedure

## Step 1 — Install Maven on the Jenkins Server

Connect to the Jenkins EC2 instance and install Maven:

```bash id="3f8m2q"
sudo yum install maven -y
```

Verify Maven installation:

```bash id="6n9p4x"
mvn --version
```

Example output:

```text
Apache Maven 3.x.x
Java version: 11
```

---

## Step 2 — Configure Git in Jenkins

Open the Jenkins Web UI.

Navigate to:

```text id="7m3v8k"
Manage Jenkins → Global Tool Configuration
```

Configure the Git section:

| Setting  | Value          |
| -------- | -------------- |
| **Name** | `Default Git`  |
| **Path** | `/usr/bin/git` |

---

## Step 3 — Configure Maven in Jenkins

In:

```text id="4x8q2m"
Manage Jenkins → Global Tool Configuration
```

Navigate to the Maven section.

Configure Maven:

| Setting                   | Value       |
| ------------------------- | ----------- |
| **Installation Name**     | `Maven-3.8` |
| **Install Automatically** | Enabled     |

Jenkins will automatically download and configure Maven.

---

# Step 4 — Create a Jenkins Freestyle Build Job

Create a new Jenkins job:

1. Select:

   ```text
   New Item
   ```

2. Enter:

   ```text
   Java-CI-Build
   ```

3. Select:

   ```text
   Freestyle project
   ```

4. Click **OK**.

---

## Step 5 — Configure Source Code Management

Under:

```text id="9v5k3p"
Source Code Management
```

Select:

```text id="2h7m9x"
Git
```

Provide your repository URL:

```text
Your GitHub Repository URL
```

Example:

```text
git@github.com:YOUR_USERNAME/your-java-project.git
```

---

## Step 6 — Configure Maven Build Step

Navigate to:

```text id="5n8q4r"
Build Steps → Add build step → Invoke top-level Maven targets
```

Configure Maven goals:

```bash id="1p6x9w"
clean package
```

---

## Step 7 — Run the Build

1. Select:

   ```text
   Save
   ```

2. Select:

   ```text
   Build Now
   ```

3. Open:

   ```text
   Console Output
   ```

4. Verify the build completes successfully.

---

# Sample Maven Project Structure

Create a sample Java application:

```bash id="8r4m6k"
mkdir -p ~/java-app/src/main/java
cd ~/java-app
```

Generate a Maven project:

```bash id="6w2p9n"
mvn archetype:generate \
-DgroupId=com.devops \
-DartifactId=myapp \
-DarchetypeArtifactId=maven-archetype-quickstart \
-DinteractiveMode=false
```

Example Maven project structure:

```text
myapp/
├── pom.xml
└── src/
    ├── main/
    │   └── java/
    └── test/
        └── java/
```

---

# CI Pipeline Workflow

The Jenkins CI workflow follows this process:

```text id="3j8v5m"
Developer Commit
        |
        ↓
GitHub Repository
        |
        ↓
Jenkins Job Trigger
        |
        ↓
Git Checkout
        |
        ↓
Maven clean package
        |
        ↓
Build Artifact (.jar)
```

---

# Best Practice Tips

> [!TIP]
> Apply these recommendations to improve Jenkins CI reliability, security, and maintainability.

## 🧹 Use `clean package`

Always use:

```bash id="8p4v2s"
clean package
```

instead of only:

```bash id="4k7n9m"
package
```

Benefits:

* Removes old build artifacts.
* Prevents stale files from affecting builds.
* Ensures a clean build environment.

---

## 🔐 Store Maven Credentials Securely

For Maven repository authentication:

* Store `settings.xml` securely.
* Store Nexus/Artifactory credentials in the Jenkins Credentials Store.

Avoid:

* Hardcoding usernames.
* Storing passwords in source code.

---

## 📄 Prefer Jenkins Pipeline Over Freestyle Jobs

Use:

```text
Jenkinsfile
```

instead of only Freestyle jobs.

Benefits:

* Pipeline configuration is stored as code.
* Version controlled with the application source.
* Easier collaboration and auditing.

---

## 📦 Archive Build Artifacts

After successful builds:

Navigate to:

```text id="0m5x7q"
Post-build Actions → Archive the artifacts
```

Configure artifact path:

```text id="5v8k2n"
target/*.jar
```

This stores generated application packages in Jenkins.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Maven installed on Jenkins server.
* ✅ Maven version verified.
* ✅ Git configured in Jenkins.
* ✅ Maven configured in Jenkins.
* ✅ Freestyle CI job created.
* ✅ GitHub repository connected.
* ✅ Maven `clean package` build executed.
* ✅ Build output verified successfully.
* ✅ Java artifact generated.

---

# Summary

In this lab, you:

* Installed Maven on Jenkins.
* Configured Git and Maven build tools.
* Created a Jenkins CI job.
* Connected Jenkins with GitHub.
* Executed a Maven Java build.
* Learned CI/CD best practices including artifact archiving and pipeline-as-code.

You are now ready to continue with the next lab in the **Git & GitHub | CI/CD With Jenkins** learning path.
