etcd-Operator Instance can be created using the Custom Resource Definition YAML files.

## Create a custom resource YAML file

```execute
cat <<'EOF' >etcd-cluster.yaml
apiVersion: etcd.database.coreos.com/v1beta2
kind: EtcdCluster
metadata:
  name: example
spec:
  size: 3
  version: 3.2.13
EOF
```

## create etcd-cluster in the same namespace of operator.

```execute
kubectl create -f etcd-cluster.yaml -n my-etcd
```

## Check etcd-cluster pods.

```execute
kubectl get pods -n my-etcd
```

It may take up to a few minutes until all the resources are created and the cluster is ready for use.


**Note - Please wait till `Status` will be `Running` and `READY` should be 1/1 , and then proceed further.**

# Accessing etcd-cluster from within a pod/container

Open the cluster-service

```execute
kubectl get svc -n my-etcd
```

To access etcd cluster externally, first update service to use NodePort:

# Execute below command to use NodePort:
```execute
kubectl get service example-client --output yaml -n my-etcd > /tmp/my-etcd.yaml
sed -i "s/type: .*/type: NodePort/g" /tmp/my-etcd.yaml
kubectl patch svc example-client -p "$(cat /tmp/my-etcd.yaml)" --namespace my-etcd
```

# Execute below command to update NodePort to 32379:
```execute
kubectl get service example-client --output yaml -n my-etcd > /tmp/my-etcd.yaml
sed -i "s/nodePort: .*/nodePort: 32379/g" /tmp/my-etcd.yaml
kubectl patch svc example-client -p "$(cat /tmp/my-etcd.yaml)" --namespace my-etcd
```


 Connect to etcd-cluster from etcd-client container

```execute
kubectl run --rm -i --tty etcd-client --image quay.io/coreos/etcd --restart=Never  --env ClusterIP=##ssh.host##  -- /bin/sh
```

You will see output similar below:

```
If you don't see a command prompt, try pressing enter.
/ #
```

 Put sample key-value pair into etcd-cluster database

Execute below command to put key-value pair.

```execute
ETCDCTL_API=3 etcdctl --endpoints http://##ssh.host##:32379 put cluster admin
```

Sample output:

```
OK
```

 Retrieving the value by key

Execute below command to retrive value of key

```execute
ETCDCTL_API=3 etcdctl --endpoints http://##ssh.host##:32379 get cluster 
```

Sample Output:

```
cluster
admin
```

Execute below command to exit out of terminal:

```execute
exit
```
