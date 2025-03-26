# Add Helm Repositories
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add fluent https://fluent.github.io/helm-charts
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
helm repo update

```
# Deploy Each Component with Helm
## Deploy Prometheus, AlertManager, and Grafana
### Deploy Prometheus, AlertManager, and Grafana
```bash
helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring --create-namespace
```
## Deploy Fluent Bit for Log Collection
```bash
helm install fluent-bit fluent/fluent-bit --namespace monitoring
```
##  Deploy OpenTelemetry Collector
```bash
helm install otel-collector open-telemetry/opentelemetry-collector --namespace monitoring
```
# Expose Services for Access
```bash
kubectl port-forward svc/prometheus-grafana 3000:80 -n monitoring   # Grafana
kubectl port-forward svc/prometheus-kube-prometheus 9090:9090 -n monitoring  # Prometheus
kubectl port-forward svc/prometheus-kube-alertmanager 9093:9093 -n monitoring  # AlertManager

```
Grafana Dashboard: http://localhost:3000 

Prometheus UI: http://localhost:9090

Alertmanager UI: http://localhost:9093












