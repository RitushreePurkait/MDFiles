
### Create this CR which will create a Grafana Instance  

```execute
cat <<'EOF' > GrafanaInstance.yaml
apiVersion: integreatly.org/v1alpha1
kind: Grafana
metadata:
  name: example-grafana
spec:
  ingress:
    enabled: true
  config:
    auth:
      disable_signout_menu: true
    auth.anonymous:
      enabled: true
    log:
      level: warn
      mode: console
    security:
      admin_password: secret
      admin_user: root
  dashboardLabelSelector:
    - matchExpressions:
        - key: app
          operator: In
          values:
            - grafana
EOF
```

Execute below command to create Grafana instance 

```execute
kubectl create -f GrafanaInstance.yaml -n my-grafana-operator
```

Get the associated Pods:

```execute
kubectl get pods -n my-grafana-operator
```
### Create this for Grafana Service of type NodePort 

```execute
cat <<'EOF' > GrafanaService.yaml
apiVersion: v1
kind: Service
metadata:
  name: grafana-svc
spec:
  type: NodePort
  ports:
  - name: grafana
    nodePort: 30200
    port: 3000
    protocol: TCP
    targetPort: grafana-http
  selector:
    app: grafana
EOF
```

Execute below command to create Grafana Service

```execute
kubectl create -f GrafanaService.yaml -n my-grafana-operator
```

Access the service :

```
http://<IPaddress>:30200
```