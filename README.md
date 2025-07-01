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

##  How to Deploy

### 1. Start Minikube

```bash
minikube start


## Apply Monitoring Namespace & Resources

kubectl create namespace monitoring

kubectl apply -f loki-service.yaml -n monitoring
kubectl apply -f grafana-patched.yaml -n monitoring
kubectl apply -f loki-datasource-config.yaml -n monitoring
