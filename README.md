#  Kubernetes Log Monitoring with Loki + Grafana

A simple, production-inspired log monitoring stack using **Grafana**, **Loki**, and **Promtail** — all running inside **Minikube**.  
This project helps visualize Kubernetes logs (e.g., unauthorized access, resource abuse) in near real-time using Grafana dashboards.

---

##  Components

- **Grafana**: Dashboard for querying and visualizing logs.
- **Loki**: Log aggregator (like Elasticsearch for logs).
- **Promtail**: Log shipper agent installed as a DaemonSet.
- **Minikube**: Local single-node Kubernetes cluster for development.

---

##  Files Included

| File Name                  | Description |
|---------------------------|-------------|
| `grafana-patched.yaml`    | Modified Grafana deployment with Loki source |
| `loki-service.yaml`       | Loki service and deployment |
| `loki-datasource-config.yaml` | Grafana datasource config to connect with Loki |

---
##  how the system works

<img width="745" height="257" alt="Screenshot 2025-07-11 at 11 05 31 AM" src="https://github.com/user-attachments/assets/e376f153-ef21-4647-8efc-3846a34416e0" />

### Promtail – The Log Collector
Installed on each node (or container)
Reads logs from /var/log/containers/*.log or via Kubernetes API

Adds metadata to each log line:
- namespace
- pod_name
- container_name
- node_name

### Loki – The Log Aggregator (Like ELK but lightweight)

- Loki receives all the logs sent by Promtail
- Stores logs in indexed format by labels (e.g., namespace, pod, job)
- Supports LogQL, Loki’s query language, for efficient search and filtering
  

### Grafana – Visualization & Alerting
- Grafana connects to Loki as a data source
- You write LogQL queries in Grafana to:
-   Search logs per team
-   Detect suspicious patterns
-   Visualize logs in tables or graphs
-   Create alerts on specific events
-   Pushes logs to Loki over HTTP

##  How to Deploy

### 1. Start Minikube

```bash
minikube start


## Apply Monitoring Namespace & Resources

kubectl create namespace monitoring

kubectl apply -f loki-service.yaml -n monitoring
kubectl apply -f grafana-patched.yaml -n monitoring
kubectl apply -f loki-datasource-config.yaml -n monitoring
