# 1️⃣ Expose Prometheus as a Data Source in Grafana
## Run the following command to port-forward Prometheus:
```sh
kubectl port-forward -n monitoring svc/prometheus-server 9090:9090
```
in Grafana Dashboard 

Go to Configuration → Data Sources.

Click Add data source and select Prometheus.

Set the URL to http://prometheus-server.monitoring:9090.

## 2️⃣ Create a New Dashboard in Grafana

### Click Add a new panel.

 #### Use the following PromQL queries for the key metrics.

✅ API Request Duration (Average)
Query:
``` promql 
api:request_duration_seconds:avg
```
Visualization: Line Chart

Title: "API Request Duration (Avg) - Last 5 mins"

Threshold: Warning at 0.5s, Critical at 1s

✅ API Error Rate
Query:
``` promql 
api:error_rate
```
Visualization: Gauge

Title: "API Error Rate (%)"

Threshold: Warning at 3%, Critical at 5%

✅ API 99th Percentile Latency
Query:
``` promql
api:latency_p99
```
Visualization: Line Chart

Title: "API P99 Latency"

Threshold: Warning at 0.8s, Critical at 1s

✅ API CPU Usage
Query:

``` promql
api:cpu_usage
```
Visualization: Line Chart

Title: "API CPU Usage (%)"

Threshold: Warning at 70%, Critical at 85%

✅ API Memory Usage
Query:
``` promql
api:memory_usage
```
Visualization: Gauge

Title: "API Memory Usage (%)"

Threshold: Warning at 70%, Critical at 85%

3️⃣ Save & Share the Dashboard
Click Save Dashboard and give it a meaningful name like "API Observability Dashboard".

