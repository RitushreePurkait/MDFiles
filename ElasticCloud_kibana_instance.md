# Specify a Kibana instance and associate it with your Elasticsearch cluster:

```execute
cat <<'EOF' > kibana_instance.yaml
apiVersion: kibana.k8s.elastic.co/v1beta1
kind: Kibana
metadata:
  name: kibanainstance
spec:
  version: 7.4.2
  count: 1
  elasticsearchRef:
    name: elasticsearch
EOF
```

# Execute below command to create kibana instance

```execute
kubectl create -f kibana_instance.yaml -n operators
```

# Monitor Kibana health and creation progress.

Similar to Elasticsearch, you can retrieve details about Kibana instances:

```execute
kubectl get kibana -n operators
```

And the associated Pods:

```execute
kubectl get pod -n operators --selector='kibana.k8s.elastic.co/name=kibanainstance'
```

**Note - Please wait till `Status` should be `Running` and `READY` should be 1/1 , and then proceed further.**

# Access Kibana.

To access Kibana service externally, update it to use NodePort:

```execute
kubectl get service kibanainstance-kb-http -n operators --output yaml > /tmp/kb_test.yaml
sed -i "s/type: .*/type: NodePort/g" /tmp/kb_test.yaml
kubectl patch svc kibanainstance-kb-http -n operators -p "$(cat /tmp/kb_test.yaml)"
```

Execute below command to get Kibana Service details:

```execute
kubectl get service kibanainstance-kb-http -n operators
```

Sample output:
```
NAME                     TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kibanainstance-kb-http   NodePort   10.96.67.171   <none>        5601:30449/TCP   7m42s
```

You need to login as the **elastic** user via browser access and The password can be obtained with the following command:

```execute
kubectl get secret elasticsearch-es-elastic-user -n operators -o=jsonpath='{.data.elastic}' | base64 --decode; echo
```

Click on the <a href="https://##ssh.host##:##NodePort##" target="_blank">https://##ssh.host##:##NodePort##</a> to access Kibana Dashboard

