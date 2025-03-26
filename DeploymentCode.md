# Add Helm Repositories
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add fluent https://fluent.github.io/helm-charts
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
helm repo update

```
# Deploy Each Component with Helm
## Deploy Prometheus, AlertManager, and Grafana
```bash
helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring --create-namespace
```
