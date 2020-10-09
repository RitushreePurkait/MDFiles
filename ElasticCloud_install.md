## Execute below command to create directory for this operator

```execute
cd && mkdir elastic-cloud
cd elastic-cloud
```

## Execute below command to install the operator

```execute
kubectl create -f https://operatorhub.io/install/elastic-cloud-eck.yaml
```

This Operator will be installed in the "operators" namespace and will be usable from all namespaces in the cluster.

```
--- 
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata: 
  name: my-elastic-cloud-eck
  namespace: operators
  spec: 
    channel: beta
    name: elastic-cloud-eck
    source: operatorhubio-catalog
    sourceNamespace: olm
```


## Execute below command to list operator

```execute
kubectl get csv -n operators | egrep -i "name|elastic-cloud"
```

**Note - Please wait till `PHASE` status will be `Succeeded` and then proceed further.**