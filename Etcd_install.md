
## Execute below command to create directory for this operator

```execute
cd && mkdir etcd-operator
cd etcd-operator
```

## Execute below command to install the operator by running the following command:

```execute
kubectl create -f https://operatorhub.io/install/etcd.yaml
```

This Operator will be installed in the "my-etcd" namespace and will be usable from all namespaces in the cluster.

```
--- 
apiVersion: v1
kind: Namespace
metadata: 
  apiVersion: operators.coreos.com/v1alpha2
  kind: OperatorGroup
  metadata: 
    name: operatorgroup
    namespace: my-etcd
    spec: 
      apiVersion: operators.coreos.com/v1alpha1
      kind: Subscription
      metadata: 
        name: my-etcd
        namespace: my-etcd
        spec: 
          channel: singlenamespace-alpha
          name: etcd
          source: operatorhubio-catalog
          sourceNamespace: olm
      targetNamespaces: my-etcd
  name: my-etcd
```


## Execute below command to watch your operator come up .

```execute
kubectl get csv -n my-etcd | egrep -i "name|etcd"
```

**Note - Please wait till `PHASE` will be `Succeeded` and then proceed further.**