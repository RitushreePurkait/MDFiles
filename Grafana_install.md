## Execute below command to create directory for this operator

```execute
cd && mkdir Grafana-Operator
cd Grafana-Operator
```

## Execute below command to install the operator

```execute
kubectl create -f https://operatorhub.io/install/grafana-operator.yaml
```

This Operator will be installed in the "my-grafana-operator" namespace and will be usable from this namespace only.

```
apiVersion: v1 
kind: Namespace 
metadata: 
  name: my-grafana-operator 
--- 
apiVersion: operators.coreos.com/v1 
kind: OperatorGroup 
metadata: 
  name: operatorgroup 
  namespace: my-grafana-operator 
spec: 
  targetNamespaces:
  - my-grafana-operator 
---
apiVersion: operators.coreos.com/v1alpha1 
kind: Subscription 
metadata: 
  name: my-grafana-operator 
  namespace: my-grafana-operator 
spec: 
  channel: alpha 
  name: grafana-operator 
  source: operatorhubio-catalog 
  sourceNamespace: olm
```

## After install, watch your operator come up using next command.

```execute
kubectl get csv -n my-grafana-operator
```

**Note - Please wait till `PHASE` status will be `Succeeded` and then proceed further.**