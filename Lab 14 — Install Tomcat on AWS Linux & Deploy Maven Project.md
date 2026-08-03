# Lab 14 — Install Tomcat on AWS Linux & Deploy Maven Project

> [!IMPORTANT]
> This lab demonstrates how to deploy a Maven-generated **WAR artifact** to Apache Tomcat using a Jenkins post-build deployment action.

## Objective

Deploy a Maven WAR artifact to Tomcat via Jenkins post-build action.

---

## Prerequisites

Before you begin, ensure that you have:

* ✅ Completed **Lab 13 — Install Git & Maven on Jenkins for Continuous Integration**.
* ✅ A Jenkins server with a successful Maven build job.
* ✅ An AWS Linux EC2 instance available for Tomcat deployment.
* ✅ A Maven project that generates a WAR artifact.

---

# Procedure

## Step 1 — Install Tomcat on AWS Linux EC2

Connect to the AWS Linux EC2 instance and install Tomcat:

```bash id="7f3n9q"
sudo yum install tomcat tomcat-webapps tomcat-admin-webapps -y
```

Start the Tomcat service:

```bash id="3x8m5p"
sudo systemctl start tomcat
```

Enable Tomcat to start automatically after reboot:

```bash id="9k4v2m"
sudo systemctl enable tomcat
```

Verify Tomcat status:

```bash id="6p8r3t"
sudo systemctl status tomcat
```

---

## Step 2 — Configure Tomcat Deployment User

Edit the Tomcat users configuration file:

```bash id="5m7q1x"
sudo nano /etc/tomcat/tomcat-users.xml
```

Add the following configuration inside the:

```xml id="0x9v4m"
<tomcat-users>
</tomcat-users>
```

tag:

```xml id="4j8n2k"
<role rolename="manager-script"/>
<user username="deployer" password="Deploy@123" roles="manager-script"/>
```

This creates a Tomcat deployment user with the required deployment role.

> [!WARNING]
> The password shown above is for lab purposes only. Use secure credentials and secret management solutions in production environments.

---

## Step 3 — Restart Tomcat

Restart Tomcat to apply the configuration changes:

```bash id="2n6w8q"
sudo systemctl restart tomcat
```

Verify the service:

```bash id="8r5m3v"
sudo systemctl status tomcat
```

---

# Step 4 — Install Jenkins Deployment Plugin

In Jenkins:

Navigate to:

```text id="5x9m7p"
Manage Jenkins → Manage Plugins → Available
```

Install:

```text id="1k7v4n"
Deploy to Container Plugin
```

Restart Jenkins if required.

---

# Step 5 — Configure Jenkins WAR Deployment

Open the Jenkins build job.

Add a post-build deployment action:

```text id="7q2m8s"
Post-build Actions → Deploy WAR
```

Configure the container:

| Setting             | Value                   |
| ------------------- | ----------------------- |
| **Container**       | Tomcat 9.x              |
| **URL**             | `http://localhost:8080` |
| **Deployment User** | `deployer`              |
| **Password**        | `Deploy@123`            |

Configure the generated WAR artifact from the Maven build output.

---

# Step 6 — Build and Verify Deployment

Run the Jenkins build:

```text id="0w6m4k"
Build Now
```

After successful deployment, access the application:

```text id="8v3p6n"
http://<EC2-IP>:8080/myapp/
```

Replace:

```text id="5h7k9m"
<EC2-IP>
```

with the public IP address of the Tomcat server.

---

# Deployment Workflow

The deployment flow follows this process:

```text id="3c7m8p"
Developer Commit
        |
        ↓
GitHub Repository
        |
        ↓
Jenkins CI Build
        |
        ↓
Maven Package
        |
        ↓
WAR Artifact
        |
        ↓
Tomcat Deployment
        |
        ↓
Application Available
```

---

# Best Practice Tips

> [!TIP]
> Apply these recommendations when implementing Tomcat deployments in real environments.

## 🖥️ Separate Jenkins and Tomcat Servers

For production environments:

* Run Jenkins and Tomcat on separate EC2 instances.
* Avoid placing build tools and application servers on the same host.

Benefits:

* Improved security isolation.
* Better resource management.
* Easier scaling.

---

## 🔒 Use HTTPS for Tomcat Manager

In production:

* Enable HTTPS for Tomcat Manager access.
* Never transmit deployment credentials over HTTP.

Avoid:

```text id="9v5m2x"
HTTP + Plain Text Credentials
```

---

## ❤️ Automate Application Health Checks

After deployment, validate application availability:

```bash id="6m8q4r"
curl -s http://localhost:8080/myapp/health | grep UP
```

This helps detect:

* Failed deployments.
* Application startup issues.
* Runtime problems.

---

## 🐳 Consider Docker-Based Deployments

Modern DevOps environments often prefer containers.

Advantages of Docker-based deployments:

* Portable application environments.
* Consistent deployments.
* Easier scaling.
* Better CI/CD integration.

---

# Validation

Verify that the following tasks have been completed successfully:

* ✅ Tomcat installed on AWS Linux EC2.
* ✅ Tomcat service started and enabled.
* ✅ Deployment user configured.
* ✅ Jenkins Deploy to Container plugin installed.
* ✅ Jenkins job configured for WAR deployment.
* ✅ Maven WAR artifact deployed successfully.
* ✅ Application accessible through Tomcat URL.

---

# Summary

In this lab, you:

* Installed and configured Apache Tomcat.
* Created a Tomcat deployment user.
* Integrated Jenkins with Tomcat.
* Deployed a Maven WAR artifact automatically.
* Verified application availability.
* Learned best practices for production Tomcat deployments.

You are now ready to continue with the next lab in the **Git & GitHub | CI/CD With Jenkins** learning path.
