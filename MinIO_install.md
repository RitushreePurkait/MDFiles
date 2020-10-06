## Execute below command to create directory for this operator

```execute
cd && mkdir MinIO-Operator
cd MinIO-Operator
```

## Execute below command to install the operator

```execute
kubectl create -f https://operatorhub.io/install/minio-operator.yaml
```

This Operator will be installed in the "my-minio-operator" namespace and will be usable from this namespace only.

```
apiVersion: v1 
kind: Namespace 
metadata: 
  name: my-minio-operator 
--- 
apiVersion: operators.coreos.com/v1 
kind: OperatorGroup 
metadata: 
  name: operatorgroup 
  namespace: my-minio-operator 
spec: 
  targetNamespaces:
  - my-minio-operator 
---
apiVersion: operators.coreos.com/v1alpha1 
kind: Subscription 
metadata: 
  name: my-minio-operator 
  namespace: my-minio-operator 
spec: 
  channel: stable 
  name: minio-operator 
  source: operatorhubio-catalog 
  sourceNamespace: olm
```

## After install, watch your operator come up using next command.

```execute
kubectl get csv -n my-minio-operator
```

**Note - Please wait till `PHASE` status will be `Succeeded` and then proceed further.**