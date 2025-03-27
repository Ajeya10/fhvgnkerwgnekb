
# 1. Kubernetes Namespace & Installation
## Created monitoring namespace

### Installed Prometheus, Grafana, and Alertmanager using Helm or manifests

``` sh
kubectl create namespace monitoring

```
##2. Add the Helm Repository:

``` sh
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

```
## 3. Install Prometheus, Grafana, and Alertmanager in monitoring Namespace:

``` sh
helm install prometheus-stack prometheus-community/kube-prometheus-stack --namespace monitoring

```
#### ✅Prometheus (for metrics collection)
### ✅ Grafana (for visualization)
### ✅ Alertmanager (for handling alerts)



