# Atlan Observability Setup

## 📌 Overview
This repository provides a complete setup for observability using **Prometheus, Grafana, Fluent Bit, OpenTelemetry, and Alertmanager** in a Kubernetes cluster. This setup helps engineers quickly debug issues by monitoring API performance, infrastructure health, and logs.

## 📂 Repository Structure
```
├── helm-charts/
│   ├── prometheus/
│   ├── grafana/
│   ├── fluent-bit/
│   ├── opentelemetry/
│   ├── alertmanager/
├── manifests/
│   ├── namespace.yaml
│   ├── prometheus.yaml
│   ├── grafana.yaml
│   ├── fluent-bit.yaml
│   ├── opentelemetry.yaml
│   ├── alertmanager.yaml
├── dashboards/
│   ├── api-performance.json
│   ├── infrastructure-health.json
│   ├── logging.json
├── alerts/
│   ├── prometheus-rules.yaml
│   ├── alertmanager-config.yaml
├── README.md
```

## 🚀 Setup Guide
### 1️⃣ Prerequisites
Ensure you have the following installed:
- **Kubernetes Cluster** (Minikube, EKS, AKS, GKE, etc.)
- **Helm** (for package management)
- **kubectl** (for interacting with Kubernetes)

### 2️⃣ Create Namespaces
```bash
kubectl create namespace monitoring
kubectl create namespace app-namespace
```

### 3️⃣ Deploy Monitoring Stack
```bash
helm install prometheus helm-charts/prometheus --namespace monitoring
helm install grafana helm-charts/grafana --namespace monitoring
helm install fluent-bit helm-charts/fluent-bit --namespace monitoring
helm install opentelemetry helm-charts/opentelemetry --namespace monitoring
helm install alertmanager helm-charts/alertmanager --namespace monitoring
```

### 4️⃣ Access Grafana Dashboard
```bash
kubectl port-forward svc/grafana 3000:3000 -n monitoring
```
- Open [http://localhost:3000](http://localhost:3000)
- Default credentials: **admin/admin**
- Import dashboards from `dashboards/`

### 5️⃣ Setup Alerts
Apply Prometheus alert rules and Alertmanager config:
```bash
kubectl apply -f alerts/prometheus-rules.yaml -n monitoring
kubectl apply -f alerts/alertmanager-config.yaml -n monitoring
```

## 📊 Metrics & Logs
| **Component**    | **Purpose** |
|-----------------|-------------|
| **Prometheus**  | Collects and stores metrics |
| **Grafana**     | Visualizes metrics and logs |
| **Fluent Bit**  | Collects and forwards logs |
| **OpenTelemetry** | Traces requests across services |
| **Alertmanager** | Handles alerts from Prometheus |

## 🔍 Dashboards & Alerts
- **API Performance** (latency, error rate, request volume)
- **Infrastructure Health** (CPU, memory, pod restarts)
- **Logging & Tracing** (structured logs, error categorization)
- **Alerting** (real-time notifications on failures)

## ⚡ Future Improvements
- Automate dashboard setup with Grafana API
- Implement log sampling to reduce storage costs
- Fine-tune alert thresholds to prevent alert fatigue

---
🎯 **Contributors:** _Your Name_

🔗 **License:** MIT
