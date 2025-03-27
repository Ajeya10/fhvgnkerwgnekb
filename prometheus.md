```yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: high-latency-alert
  namespace: monitoring
spec:
  groups:
    - name: API_Health
      rules:
        - alert: HighLatency
          expr: histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m])) > 0.5
          for: 2m
          labels:
            severity: critical
          annotations:
            summary: "High API latency detected"
            description: "95th percentile response time is above 500ms."
```
