# Lab 49 — Install & Configure Grafana

## 📌 Objective

Install Grafana, configure a Prometheus data source, and build an infrastructure monitoring dashboard.

---

## 🛠️ Prerequisites

Before starting this lab, ensure:

* ✅ Prometheus is installed and running (refer to **Lab 48 — Install & Configure Prometheus**).
* ✅ EC2 instance or server is accessible.
* ✅ Port `3000` is allowed in the security group/firewall.
* ✅ Prometheus is available at:

```text
http://<EC2-IP>:9090
```

---

# 🚀 Steps

## Step 1 — Install Grafana

Install required dependencies:

```bash
sudo apt install -y apt-transport-https software-properties-common wget
```

Add the Grafana GPG key:

```bash
wget -q -O - https://apt.grafana.com/gpg.key | sudo apt-key add -
```

Add the Grafana repository:

```bash
echo "deb https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
```

Update package information:

```bash
sudo apt update
```

Install Grafana:

```bash
sudo apt install -y grafana
```

Start and enable the Grafana service:

```bash
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

Verify Grafana status:

```bash
sudo systemctl status grafana-server
```

---

# 🌐 Step 2 — Access Grafana Web Interface

Open the Grafana dashboard:

```text
http://<EC2-IP>:3000
```

Default credentials:

```text
Username: admin
Password: admin
```

⚠️ **Security Recommendation**

After the first login:

* Change the default administrator password.
* Enable authentication controls before production usage.

---

# 📊 Step 3 — Add Prometheus Data Source

In Grafana:

1. Navigate to:

```
Configuration → Data Sources
```

2. Select:

```
Add Data Source
```

3. Choose:

```
Prometheus
```

4. Configure the Prometheus URL:

```text
http://localhost:9090
```

5. Click:

```
Save & Test
```

Expected result:

```
Data source is working
```

---

# 📈 Step 4 — Import Infrastructure Dashboard

Navigate to:

```
Dashboards → Import
```

Enter dashboard ID:

```text
1860
```

Dashboard:

```
Node Exporter Full
```

Click:

```
Import
```

This dashboard provides:

* CPU utilization monitoring
* Memory usage
* Disk utilization
* Network statistics
* Host-level metrics

---

# 📉 Step 5 — Create Custom Grafana Panel

In Grafana panel editor:

## Metric Query

```promql
rate(http_requests_total[5m])
```

## Legend

```text
{{handler}} - {{method}}
```

## Panel Configuration

```text
Panel Type:
Time series
```

## Threshold Configuration

| Level    |            Value |
| -------- | ---------------: |
| Warning  | 100 requests/sec |
| Critical | 500 requests/sec |

---

# 💡 Best Practice Tips

> 💡 **Dashboard Version Control**

Use Grafana dashboard version control:

* Export dashboard JSON.
* Commit dashboard files to Git.
* Review dashboard changes through pull requests.

---

> 🔔 **Alert Integration**

Grafana alerts can be integrated with:

* Slack
* PagerDuty
* Email
* Other notification channels

Configure notification channels for production monitoring.

---

> 📚 Recommended Dashboard Templates

| Dashboard ID | Purpose                                 |
| ------------ | --------------------------------------- |
| `315`        | Docker Monitoring                       |
| `6417`       | Kubernetes Cluster Monitoring           |
| `1860`       | Node Exporter Infrastructure Monitoring |

---

> 🎯 **Dynamic Dashboards**

Use Grafana variables:

* Create instance dropdowns.
* Switch between servers dynamically.
* Reuse dashboards across multiple environments.

---

## ✅ Lab Completion Checklist

* [ ] Grafana installed successfully.
* [ ] Grafana service enabled and running.
* [ ] Prometheus connected as a data source.
* [ ] Node Exporter dashboard imported.
* [ ] Custom monitoring panel created.
* [ ] Dashboard best practices reviewed.

---

## 📖 Next Lab

➡️ Continue with the next observability lab to configure centralized logging and production monitoring practices.
