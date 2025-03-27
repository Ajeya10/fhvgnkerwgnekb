# establishing connection between RESTAPI and Prometheus 

##If your API is in Python (Flask/Django), use Prometheus client library:
```js
from prometheus_client import start_http_server, Counter, Gauge
import random, time

# Define a custom metric
REQUEST_COUNT = Counter("api_requests_total", "Total API requests")
LATENCY = Gauge("api_latency_seconds", "API response time in seconds")

def process_request():
    REQUEST_COUNT.inc()
    latency = random.uniform(0.1, 1.5)
    LATENCY.set(latency)
    time.sleep(latency)

if __name__ == "__main__":
    start_http_server(8000)  # Exposes metrics at /metrics on port 8000
    while True:
        process_request()

```



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
