## Execute below command to create directory for this operator

```execute
cd && mkdir Prometheus-Operator
cd Prometheus-Operator
```

## Execute below command to install the operator

```execute
kubectl create -f https://operatorhub.io/install/prometheus.yaml
```

This Operator will be installed in the "operators" namespace and will be usable from all namespaces in the cluster.

```
apiVersion: operators.coreos.com/v1alpha1 
kind: Subscription 
metadata: 
  name: my-prometheus 
  namespace: operators 
spec: 
  channel: beta 
  name: prometheus 
  source: operatorhubio-catalog 
  sourceNamespace: olm
```

## After install, watch your operator come up using next command.

```execute
kubectl get csv -n operators
```

**Note - Please wait till `PHASE` status will be `Succeeded` and then proceed further.**