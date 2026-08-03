# Lab 50 — Integrate Prometheus & Grafana with Kubernetes

## 📌 Objective

Deploy the Prometheus monitoring stack on Kubernetes using Helm and monitor Kubernetes cluster metrics through Grafana dashboards.

---

## 🛠️ Prerequisites

Before starting this lab, ensure:

* ✅ A running Kubernetes cluster is available.
* ✅ `kubectl` is configured on the Kubernetes master/control-plane node.
* ✅ Helm is installed or ready to be installed.
* ✅ Cluster nodes have sufficient resources for monitoring components.

---

# 🚀 Steps

## Step 1 — Install Helm on Master Node

Install Helm using the official installation script:

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

Verify Helm installation:

```bash
helm version
```

Expected output:

```text
Helm version information
```

---

# Step 2 — Add Prometheus Community Helm Repository

Add the Prometheus community Helm chart repository:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

Update Helm repositories:

```bash
helm repo update
```

---

# Step 3 — Install kube-prometheus-stack

The `kube-prometheus-stack` package installs:

* 📊 Prometheus
* 📈 Grafana
* 🚨 Alertmanager
* 🖥️ Node Exporter
* 🔍 Kubernetes monitoring components

Create a dedicated monitoring namespace:

```bash
kubectl create namespace monitoring
```

Install the monitoring stack:

```bash
helm install prometheus-stack prometheus-community/kube-prometheus-stack \
--namespace monitoring \
--set grafana.adminPassword=Admin@123 \
--set prometheus.prometheusSpec.retention=7d
```

Verify monitoring pods:

```bash
kubectl get pods -n monitoring
```

Expected components:

```text
prometheus-stack-grafana
prometheus-stack-kube-prometheus
prometheus-stack-alertmanager
prometheus-stack-prometheus-node-exporter
```

---

# Step 4 — Access Grafana Dashboard

Use Kubernetes port forwarding:

```bash
kubectl port-forward -n monitoring svc/prometheus-stack-grafana 3000:80
```

Access Grafana:

```text
http://localhost:3000
```

or:

```text
http://<NODE-IP>:3000
```

Login credentials:

```text
Username:
admin

Password:
Admin@123
```

⚠️ **Security Recommendation**

Change the default Grafana administrator password before using this setup in production environments.

---

# Step 5 — Explore Kubernetes Dashboards

The Helm deployment includes pre-built Kubernetes dashboards.

Navigate to:

```text
Grafana → Dashboards
```

Explore:

```text
Kubernetes Cluster Overview
```

Available monitoring views include:

* 🖥️ Node CPU and memory usage
* 📦 Pod resource consumption
* ☸️ Kubernetes workload health
* 🌐 Network metrics
* 🚨 Alert status

---

# 💡 Best Practice Tips

> 💡 **Use kube-prometheus-stack for Standard Kubernetes Monitoring**

`kube-prometheus-stack` is an industry-standard monitoring solution that includes:

* 30+ pre-built dashboards
* Prometheus Operator
* Alertmanager integration
* Kubernetes service discovery

---

> 💾 **Configure Persistent Storage**

Configure PersistentVolumeClaims (PVCs) for Prometheus storage:

* Prevent metric loss after pod restarts.
* Maintain historical monitoring data.
* Support long-term observability requirements.

---

> ⚙️ **Set Resource Limits**

Prometheus can consume significant memory on large Kubernetes clusters.

Always configure:

* CPU requests
* CPU limits
* Memory requests
* Memory limits

Example:

```yaml
resources:
  requests:
    memory: "2Gi"
    cpu: "500m"
  limits:
    memory: "4Gi"
    cpu: "1"
```

---

> 🔍 **Use ServiceMonitor for Custom Metrics**

Use the Kubernetes `ServiceMonitor` Custom Resource Definition (CRD):

* Automatically discovers application metrics.
* Avoids manual `prometheus.yml` modifications.
* Provides Kubernetes-native monitoring configuration.

---

# ✅ Lab Completion Checklist

* [ ] Helm installed successfully.
* [ ] Prometheus community repository added.
* [ ] kube-prometheus-stack deployed successfully.
* [ ] Monitoring namespace created.
* [ ] Prometheus, Grafana, and Alertmanager pods running.
* [ ] Grafana accessed successfully.
* [ ] Kubernetes dashboards reviewed.

---

## 📖 Next Lab

➡️ Continue to the final observability lab to complete the monitoring and logging learning path.
