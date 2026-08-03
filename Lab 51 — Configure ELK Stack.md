# Lab 51 — Configure ELK Stack

## 📌 Objective

Deploy Elasticsearch, Logstash, and Kibana (ELK Stack) for centralized log management and log analysis.

---

## 🛠️ Prerequisites

Before starting this lab, ensure:

* ✅ Ubuntu EC2 instance is available.
* ✅ Instance has sufficient resources for ELK components.
* ✅ Required ports are accessible:

  * `9200` — Elasticsearch API
  * `5601` — Kibana Web Interface
* ✅ Administrative access with `sudo` privileges is available.

---

# 🚀 Steps

## Step 1 — Provision Server for ELK Stack

For this lab environment, use:

```text
Recommended Instance:
EC2 t3.medium

Resources:
- 2 vCPU
- 4 GB RAM
```

⚠️ **Resource Note**

The ELK Stack is resource-intensive. Smaller instances may experience performance issues during indexing and dashboard usage.

---

# Step 2 — Install Elasticsearch

## Add Elasticsearch Repository Key

```bash
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg
```

Add Elasticsearch package repository:

```bash
echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
```

Update package information:

```bash
sudo apt update
```

Install Elasticsearch:

```bash
sudo apt install -y elasticsearch
```

---

## Configure Elasticsearch

Edit Elasticsearch configuration:

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

Configure single-node settings:

```yaml
network.host: localhost
```

For lab environments only, disable security:

```yaml
xpack.security.enabled: false
```

⚠️ **Security Warning**

Disabling Elasticsearch security is acceptable for learning environments only.

Production environments should enable:

* Authentication
* TLS encryption
* Role-based access control

---

## Start Elasticsearch Service

Start Elasticsearch:

```bash
sudo systemctl start elasticsearch
```

Enable Elasticsearch on boot:

```bash
sudo systemctl enable elasticsearch
```

Verify Elasticsearch:

```bash
curl -X GET http://localhost:9200
```

Expected response:

```json
{
  "name": "elasticsearch",
  "cluster_name": "elasticsearch"
}
```

---

# Step 3 — Install Kibana

Install Kibana:

```bash
sudo apt install -y kibana
```

Edit Kibana configuration:

```bash
sudo nano /etc/kibana/kibana.yml
```

Configure Kibana:

```yaml
server.host: "0.0.0.0"
```

Configure Elasticsearch connection:

```yaml
elasticsearch.hosts: ["http://localhost:9200"]
```

---

## Start Kibana Service

Start Kibana:

```bash
sudo systemctl start kibana
```

Enable Kibana on boot:

```bash
sudo systemctl enable kibana
```

Access Kibana:

```text
http://<EC2-IP>:5601
```

---

# Step 4 — Install Filebeat for Log Shipping

Install Filebeat:

```bash
sudo apt install -y filebeat
```

Enable Nginx log collection module:

```bash
sudo filebeat modules enable system nginx
```

Configure Filebeat index management:

```bash
sudo filebeat setup \
--index-management \
-E output.logstash.enabled=false \
-E output.elasticsearch.hosts=['localhost:9200']
```

Start Filebeat:

```bash
sudo systemctl start filebeat
```

Enable Filebeat on boot:

```bash
sudo systemctl enable filebeat
```

---

# Step 5 — Explore Logs in Kibana

Open Kibana:

```text
http://<EC2-IP>:5601
```

Navigate to:

```text
Kibana → Discover
```

Create an index pattern:

```text
filebeat-*
```

Explore:

* Application logs
* System logs
* Nginx access logs
* Error events
* Log search and filtering

---

# 💡 Best Practice Tips

> 💡 **Production Elasticsearch Architecture**

A single-node Elasticsearch deployment is suitable for:

* Labs
* Development
* Testing environments

Production environments should use:

* Minimum 3-node Elasticsearch cluster
* High availability configuration
* Dedicated storage nodes where required

---

> 📦 **Use Filebeat for Lightweight Log Shipping**

For simple log forwarding:

* Prefer Filebeat over Logstash.
* Filebeat consumes fewer resources.
* Filebeat provides built-in modules for common services.

Use Logstash when advanced processing is required:

* Data transformation
* Filtering
* Enrichment
* Complex routing

---

> 🗂️ **Configure Index Lifecycle Management (ILM)**

Use ILM policies to:

* Rotate indexes automatically.
* Control storage usage.
* Delete old data.

Recommended example:

```text
Delete indexes older than 30 days
```

---

> ☁️ **AWS Managed Alternative**

For AWS production environments:

Use:

```text
Amazon OpenSearch Service
```

instead of maintaining self-managed Elasticsearch clusters.

Benefits:

* Managed infrastructure
* Automated patching
* Scaling support
* AWS security integration

---

# ✅ Lab Completion Checklist

* [ ] ELK server provisioned with adequate resources.
* [ ] Elasticsearch installed and running.
* [ ] Elasticsearch API tested successfully.
* [ ] Kibana installed and accessible.
* [ ] Filebeat configured for log shipping.
* [ ] Logs visible in Kibana Discover.
* [ ] Centralized logging workflow verified.

---

# 🎉 DevOps Learning Path Completion

You have completed the complete hands-on DevOps learning journey covering:

* ☁️ AWS & Azure Cloud
* 🐧 Linux Administration
* 📝 Bash Automation
* 🔧 Git & CI/CD
* 🐳 Docker
* ☸️ Kubernetes
* ⚙️ Ansible
* 🏗️ Terraform
* 🚀 Azure DevOps
* 🔄 AWS DevOps Services
* 📊 Prometheus & Grafana
* 📚 Centralized Logging with ELK

This documentation can now serve as a GitHub-ready DevOps laboratory workbook and reference manual.
