## README.md

# Atlan Observability Challenge 2025 - Monitoring Stack 🚀

This repository contains the **Prometheus-Grafana-Alertmanager** stack deployed in a Kubernetes **monitoring namespace** to collect API metrics, visualize performance, and trigger alerts. 

## 📌 Features
- **API Monitoring** using Prometheus
- **Visual Dashboards** in Grafana
- **Alerts via Email** using Alertmanager
- **Automated Deployment with Kubernetes Manifests**

## 🏗 Deployment Steps

### **1️⃣ Clone the Repository**
```sh
git clone https://github.com/your-username/Atlan-Observability.git
cd Atlan-Observability
```

### **2️⃣ Deploy Monitoring Stack**
```sh
kubectl apply -f manifests/
```

### **3️⃣ Access Grafana Dashboard**
```sh
kubectl port-forward svc/grafana 3000:3000 -n monitoring
```
- Open **http://localhost:3000**
- Default login: `admin` / `admin`

### **4️⃣ Verify Prometheus Targets**
```sh
kubectl port-forward svc/prometheus 9090:9090 -n monitoring
```
- Open **http://localhost:9090/targets** → Ensure API is being scraped.

### **5️⃣ Test Alerts in Alertmanager**
```sh
kubectl port-forward svc/alertmanager 9093:9093 -n monitoring
```
- Open **http://localhost:9093** and check active alerts.

---

## 🚀 Additional Resources
- **Grafana Dashboards JSON files** available in `/dashboards/`
- **Setup Guide** in `/docs/setup-guide.md`
- **Video Walkthrough** (link to your recorded video)

---

## **📢 Final Notes**
This repo contains all necessary configurations to deploy **Prometheus, Grafana, and Alertmanager** to monitor REST API performance. 🎯
