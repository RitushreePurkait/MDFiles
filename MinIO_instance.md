Execute below command to create a secret with proper accesskey and secretkey

```execute
kubectl create secret generic minio-creds-secret --from-literal=accesskey=admin --from-literal=secretkey=secret@123456 --namespace my-minio-operator
```
### Create this CR which will create a MinIO Instance and its 4 replicas

```execute
cat <<'EOF' > MinIOInstance.yaml
apiVersion: miniocontroller.min.io/v1beta1
kind: MinIOInstance
metadata:
  name: minio
spec:
  replicas: 4
  credsSecret:
    name: minio-creds-secret
  requestAutoCert: false
EOF
```

Execute below command to create MinIO instance 

```execute
kubectl create -f MinIOInstance.yaml -n my-minio-operator
```

Get the associated Pods:

```execute
kubectl get pods -n my-minio-operator
```
