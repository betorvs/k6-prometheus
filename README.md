# k6-prometheus

Sources [k6-operator](https://github.com/grafana/k6-operator) and [k6](https://github.com/grafana/k6)

```
# k6-resource-with-extensions.yml

apiVersion: k6.io/v1alpha1
kind: K6
metadata:
  name: k6-sample-with-extensions
spec:
  parallelism: 4
  script:
    configMap:
      name: crocodile-stress-test
      file: test.js
  arguments: --out prometheus
  ports:
  - containerPort: 5656
    name: metrics
  image: betorvs/k6-prometheus:v0.37.0
```

Pod Monitor:
```
apiVersion: monitoring.coreos.com/v1
kind: PodMonitor
metadata:
  name: k6-monitor
spec:
  selector:
    matchLabels:
      app: k6
  podMetricsEndpoints:
  - port: metrics
    interval: 15s
```
