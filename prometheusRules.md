# prometheus-rules.yaml
``` yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: monitoring-rules
  namespace: monitoring
spec:
  groups:
  - name: api-performance-rules
    rules:
    - record: api:request_duration_seconds:avg
      expr: avg(rate(http_request_duration_seconds_sum[5m]) / rate(http_request_duration_seconds_count[5m]))
      labels:
        severity: warning
    - record: api:error_rate
      expr: sum(rate(http_requests_total{status=~"5.."}[5m])) / sum(rate(http_requests_total[5m]))
      labels:
        severity: critical
    - record: api:latency_p99
      expr: histogram_quantile(0.99, rate(http_request_duration_seconds_bucket[5m]))
      labels:
        severity: warning
    - record: api:cpu_usage
      expr: avg(rate(container_cpu_usage_seconds_total{container!="", pod=~".*api.*"}[5m]))
      labels:
        severity: warning
    - record: api:memory_usage
      expr: avg(container_memory_usage_bytes{container!="", pod=~".*api.*"}) / avg(machine_memory_bytes) * 100
      labels:
        severity: warning

  - name: api-alerts
    rules:
    - alert: HighErrorRate
      expr: sum(rate(http_requests_total{status=~"5.."}[5m])) / sum(rate(http_requests_total[5m])) > 0.05
      for: 2m
      labels:
        severity: critical
      annotations:
        summary: "High Error Rate"
        description: "API is experiencing more than 5% errors in the last 5 minutes."

    - alert: HighLatency
      expr: histogram_quantile(0.99, rate(http_request_duration_seconds_bucket[5m])) > 1
      for: 2m
      labels:
        severity: warning
      annotations:
        summary: "High Latency"
        description: "99th percentile request latency is above 1 second."

    - alert: HighCPUUsage
      expr: avg(rate(container_cpu_usage_seconds_total{container!="", pod=~".*api.*"}[5m])) > 0.8
      for: 2m
      labels:
        severity: warning
      annotations:
        summary: "High CPU Usage"
        description: "API container is using more than 80% CPU."

    - alert: HighMemoryUsage
      expr: avg(container_memory_usage_bytes{container!="", pod=~".*api.*"}) / avg(machine_memory_bytes) * 100 > 85
      for: 2m
      labels:
        severity: warning
      annotations:
        summary: "High Memory Usage"
        description: "API container is using more than 85% of allocated memory."
```
## Apply rule 
``` sh
kubectl apply -f prometheus-rules.yaml

```
