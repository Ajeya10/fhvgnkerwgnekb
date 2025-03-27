# Configure Alertmanager for Email Notifications
## Step 1: Create an Alertmanager ConfigMap
### Create a file alertmanager-config.yaml with the following configuration:
``` yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: alertmanager-config
  namespace: monitoring
data:
  alertmanager.yml: |
    global:
      resolve_timeout: 5m

    route:
      receiver: email-alert
      group_wait: 10s
      group_interval: 1m
      repeat_interval: 5m

    receivers:
      - name: email-alert
        email_configs:
          - to: "your-email@example.com"
            from: "alertmanager@example.com"
            smarthost: "smtp.gmail.com:587"
            auth_username: "your-email@example.com"
            auth_password: "your-email-app-password"
            require_tls: true
```

### Step 2: Apply ConfigMap
``` sh
kubectl apply -f alertmanager-config.yaml
```
## 2 Update Alertmanager Deployment
### Edit the Alertmanager deployment to mount the new ConfigMap:
``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: alertmanager
  namespace: monitoring
spec:
  replicas: 1
  selector:
    matchLabels:
      app: alertmanager
  template:
    metadata:
      labels:
        app: alertmanager
    spec:
      containers:
        - name: alertmanager
          image: prom/alertmanager
          args:
            - "--config.file=/etc/alertmanager/alertmanager.yml"  # Load config
          ports:
            - containerPort: 9093  # Default Alertmanager port
          volumeMounts:
            - name: config-volume
              mountPath: /etc/alertmanager
              subPath: alertmanager.yml
      volumes:
        - name: config-volume
          configMap:
            name: alertmanager-config  # Mount the ConfigMap as a volume

```
Apply the changes:
``` sh
kubectl apply -f alertmanager-deployment.yaml
```
## 3️ Update Prometheus to Use Alertmanager
### Edit the Prometheus configuration (prometheus.yaml) to include Alertmanager:
``` yaml
alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - "alertmanager.monitoring.svc.cluster.local:9093"
```
### Apply the updated Prometheus configuration:
``` sh
kubectl apply -f prometheus.yaml
```
