# establishing connection between RESTAPI and Prometheus 
# nsure your REST API has an endpoint that provides metrics in Prometheus format


## If you're using Flask, install prometheus_flask_exporter:
```sh
pip install prometheus-flask-exporter
```
``` py
from prometheus_flask_exporter import PrometheusMetrics
from flask import Flask

app = Flask(__name__)
metrics = PrometheusMetrics(app)

@app.route('/metrics')
def metrics_endpoint():
    return metrics.generate_latest()

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

```
## If using Node.js, use prom-client:
``` js
const express = require('express');
const client = require('prom-client');

const app = express();
const register = new client.Registry();
client.collectDefaultMetrics({ register });

app.get('/metrics', async (req, res) => {
    res.set('Content-Type', register.contentType);
    res.end(await register.metrics());
});

app.listen(5000, () => console.log('Server running on port 5000'));

```
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
# Update Prometheus Scrape Config
## Modify your Prometheus configuration file (prometheus.yml) to scrape the API metrics:
``` yml
scrape_configs:
  - job_name: 'api-metrics'
    metrics_path: '/metrics'  # API must expose metrics here
    static_configs:
      - targets: ['your-api-ip:8000']  # Replace with your API server IP & port
```

## If your API is inside Kubernetes, add the service name instead: Used my me 
``` sh
static_configs:
  - targets: ['my-api-service.monitoring.svc.cluster.local:8000']

```
