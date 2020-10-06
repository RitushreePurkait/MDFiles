
### Create this CR which will create a Prometheus Instance along with a ServiceAccount and a Service of Type NodePort  

```execute
cat <<'EOF' > prometheusInstance.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: prometheus
---
apiVersion: monitoring.coreos.com/v1
kind: Prometheus
metadata:
  name: prometheus
  labels:
    prometheus: prometheus
spec:
  replicas: 2
  serviceAccountName: prometheus
  securityContext: {}
  serviceMonitorSelector: {}
  ruleSelector: {}
  alerting:
    alertmanagers:
      - namespace: monitoring
        name: alertmanager-main
        port: web
---
apiVersion: v1
kind: Service
metadata:
  name: prometheus
spec:
  type: NodePort
  ports:
  - name: web
    nodePort: 30100
    port: 9090
    protocol: TCP
    targetPort: web
  selector:
    prometheus: prometheus
EOF
```

Execute below command to create Prometheus instance 

```execute
kubectl create -f prometheusInstance.yaml -n operators
```

Get the associated Pods:

```execute
kubectl get pods -n operators
```
Access the service :

```
http://<IPaddress>:30100
```
