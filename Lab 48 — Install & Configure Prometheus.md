# Section 10 — Observability: Monitoring & Logging

# Lab 48 — Install & Configure Prometheus

> [!IMPORTANT]
> This lab demonstrates how to deploy Prometheus on an EC2 instance, configure monitoring targets, collect infrastructure metrics, and write basic PromQL queries.

> [!NOTE]
> Prometheus is an open-source monitoring and alerting system designed for collecting time-series metrics from applications, servers, containers, and Kubernetes environments.

---

# Objective

Deploy and configure Prometheus to:

* 📊 Collect infrastructure metrics from EC2 instances.
* 🔍 Configure scrape targets.
* 🖥️ Monitor system resources using Node Exporter.
* 📈 Query metrics using PromQL.
* 🚨 Prepare a foundation for alerting and observability workflows.

---

# Prerequisites

Before starting, ensure:

* ✅ Linux EC2 instance available for Prometheus server.
* ✅ Target nodes available for monitoring.
* ✅ Security Group allows:

  * Port `9090` for Prometheus UI.
  * Port `9100` for Node Exporter metrics.
* ✅ Basic Linux administration knowledge.
* ✅ Internet access from EC2 instances.

---

# Prometheus Monitoring Architecture

```text
                     Prometheus Server
                           |
                           |
              -----------------------------
              |                           |
              ↓                           ↓
        Node Exporter               Node Exporter
        EC2 Instance                Worker Node

              |
              ↓

        Time-Series Metrics

              |
              ↓

          PromQL Queries

              |
              ↓

        Dashboards / Alerts
```

---

# Step 1 — Install Prometheus

## Create Dedicated Prometheus User

Create a system user:

```bash id="m7x3pq"
sudo useradd --no-create-home --shell /bin/false prometheus
```

Purpose:

* Runs Prometheus with least privilege.
* Improves system security.

---

# Download and Install Prometheus

Set version:

```bash id="q8v2ml"
PROM_VER="2.47.0"
```

Download package:

```bash id="r5k9xz"
wget https://github.com/prometheus/prometheus/releases/download/v${PROM_VER}/prometheus-${PROM_VER}.linux-amd64.tar.gz
```

Extract:

```bash id="t3n7bf"
tar xzf prometheus-${PROM_VER}.linux-amd64.tar.gz
```

Move binaries:

```bash id="p4c8wy"
sudo mv prometheus-${PROM_VER}.linux-amd64/prometheus /usr/local/bin/

sudo mv prometheus-${PROM_VER}.linux-amd64/promtool /usr/local/bin/
```

---

# Create Prometheus Directories

Create configuration and storage paths:

```bash id="u6j9qa"
sudo mkdir /etc/prometheus

sudo mkdir /var/lib/prometheus
```

Move supporting files:

```bash id="d2m8vk"
sudo mv prometheus-${PROM_VER}.linux-amd64/consoles /etc/prometheus/

sudo mv prometheus-${PROM_VER}.linux-amd64/console_libraries /etc/prometheus/
```

Set ownership:

```bash id="w9k3sf"
sudo chown -R prometheus:prometheus /etc/prometheus /var/lib/prometheus
```

---

# Step 2 — Configure Prometheus

Create configuration file:

```bash id="x2p7lm"
sudo nano /etc/prometheus/prometheus.yml
```

Add:

```yaml id="z5m8qv"
global:
  scrape_interval: 15s
  evaluation_interval: 15s

alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - 'localhost:9093'

scrape_configs:

  - job_name: 'prometheus'
    static_configs:
      - targets:
          - 'localhost:9090'

  - job_name: 'node_exporter'
    static_configs:
      - targets:
          - 'localhost:9100'
          - '<WORKER-IP>:9100'
```

---

# Prometheus Configuration Reference

| Configuration         | Purpose                        |
| --------------------- | ------------------------------ |
| `scrape_interval`     | Frequency of metric collection |
| `evaluation_interval` | Frequency of rule evaluation   |
| `job_name`            | Monitoring target group        |
| `targets`             | Servers exposing metrics       |

---

# Step 3 — Create Prometheus Systemd Service

Create service file:

```bash id="a7n4kp"
cat > /tmp/prometheus.service << 'EOF'

[Unit]
Description=Prometheus
After=network.target

[Service]
User=prometheus
ExecStart=/usr/local/bin/prometheus \
--config.file=/etc/prometheus/prometheus.yml \
--storage.tsdb.path=/var/lib/prometheus \
--storage.tsdb.retention.time=30d

Restart=always

[Install]
WantedBy=multi-user.target

EOF
```

Move service:

```bash id="m3q8vy"
sudo mv /tmp/prometheus.service /etc/systemd/system/
```

Reload systemd:

```bash id="b8v2qm"
sudo systemctl daemon-reload
```

Start Prometheus:

```bash id="f6x9wd"
sudo systemctl start prometheus
```

Enable startup:

```bash id="n4k7pz"
sudo systemctl enable prometheus
```

Check status:

```bash id="c9m2rx"
sudo systemctl status prometheus
```

---

# Step 4 — Install Node Exporter

> [!NOTE]
> Node Exporter runs on monitored servers and exposes Linux system metrics on port `9100`.

---

## Download Node Exporter

Set version:

```bash id="h5w8qa"
NODE_VER="1.6.1"
```

Download:

```bash id="j3p9mv"
wget https://github.com/prometheus/node_exporter/releases/download/v${NODE_VER}/node_exporter-${NODE_VER}.linux-amd64.tar.gz
```

Extract:

```bash id="k7m4xz"
tar xzf node_exporter-${NODE_VER}.linux-amd64.tar.gz
```

Move binary:

```bash id="v8q2ms"
sudo mv node_exporter-${NODE_VER}.linux-amd64/node_exporter /usr/local/bin/
```

---

## Create Node Exporter Service

Create a similar systemd service:

```text id="r9m3kw"
Service Name:

node_exporter


Port:

9100
```

Start and enable:

```bash id="p5x7mq"
sudo systemctl start node_exporter

sudo systemctl enable node_exporter
```

---

# Step 5 — Access Prometheus Web UI

Open:

```text id="z8q4mv"
http://<EC2-IP>:9090
```

Prometheus UI allows:

* Query execution.
* Target health checks.
* Metric exploration.

---

# PromQL Query Examples

## CPU Usage Percentage

```promql id="w3m8qx"
100 - (
  avg by(instance)
  (
    rate(node_cpu_seconds_total{mode="idle"}[5m])
  ) * 100
)
```

Purpose:

* Calculates CPU utilization percentage.
* Excludes idle CPU time.

---

## Memory Usage Percentage

```promql id="q6v9mz"
(
node_memory_MemTotal_bytes -
node_memory_MemAvailable_bytes
)
/
node_memory_MemTotal_bytes
* 100
```

Purpose:

* Shows used memory percentage.

---

## Disk Usage Percentage

```promql id="m4x7qa"
(
1 -
node_filesystem_avail_bytes /
node_filesystem_size_bytes
)
* 100
```

Purpose:

* Calculates filesystem utilization.

---

# Monitoring Validation Checklist

Verify:

* ✅ Prometheus user created.
* ✅ Prometheus binaries installed.
* ✅ Configuration file created.
* ✅ Prometheus service running.
* ✅ Node Exporter installed.
* ✅ Target nodes visible in Prometheus.
* ✅ PromQL queries return metrics.
* ✅ Prometheus UI accessible.

---

# Best Practice Tips

> [!TIP]
> Apply these recommendations for production monitoring environments.

---

## 💾 Configure Data Retention

Set retention:

```bash
--storage.tsdb.retention.time=30d
```

Benefits:

* Prevents unlimited disk growth.
* Controls storage usage.
* Improves maintenance.

---

## ⚡ Use Recording Rules

For complex PromQL queries:

```text id="x9m5qb"
Raw Metrics
      |
      ↓
Recording Rule
      |
      ↓
Precomputed Result
      |
      ↓
Dashboard
```

Benefits:

* Faster queries.
* Reduced Prometheus workload.

---

## 🚨 Configure Alerting Rules

Recommended alerts:

```text id="c4m8vy"
CPU Usage > 80%

Disk Usage > 85%

Pod OOMKilled

Instance Down
```

Integrate with:

* Alertmanager.
* Email.
* Slack.
* Incident management tools.

---

## 🏢 Production High Availability

For enterprise environments:

Use:

* Thanos.
* Cortex.

Benefits:

* Long-term metric storage.
* High availability Prometheus.
* Multi-cluster monitoring.

---

# Summary

In this lab, you:

* Installed Prometheus on EC2.
* Configured metric scrape targets.
* Installed Node Exporter for system metrics.
* Learned PromQL queries.
* Applied monitoring and observability best practices.

This provides the foundation for building dashboards, alerts, and production-grade observability platforms.
