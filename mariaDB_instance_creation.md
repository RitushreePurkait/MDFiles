 As Database and Backup files  will be stored at location: '/mnt/data' and '/mnt/backup'. These location should be created before applying the CR.
Execute below command to create locations and provide permissions

```execute
mkdir /mnt/data
mkdir /mnt/backup

chmod a+rwx /mnt/data
chmod a+rwx /mnt/backup
```
### Create this CR which will create a database called test-db, along with user credentials

```execute
cat <<'EOF' > MariaDBserver.yaml
apiVersion: mariadb.persistentsys/v1alpha1
kind: MariaDB
metadata:
  name: mariadb
spec:
  # Keep this parameter value unchanged.
  size: 1
  
  # Root user password
  rootpwd: password

  # New Database name
  database: test-db
  # Database additional user details (base64 encoded)
  username: db-user 
  password: db-user 

  # Image name with version
  image: "mariadb/server:10.3"

  # Database storage Path
  dataStoragePath: "/mnt/data" 

  # Database storage Size (Ex. 1Gi, 100Mi)
  dataStorageSize: "1Gi"

  # Port number exposed for Database service
  port: 30685
EOF
```

Execute below command to create MariaDBserver instance 

```execute
kubectl create -f MariaDBserver.yaml -n my-mariadb-operator-app
```
### Create this CR which will schedule backup of MariaDB at defined schedule. The Database backup files will be stored at location: '/mnt/backup'.

```execute
cat <<'EOF' > MariaDBBackup.yaml
apiVersion: mariadb.persistentsys/v1alpha1
kind: Backup
metadata:
  name: mariadb-backup
spec:
  # Backup Path
  backupPath: "/mnt/backup"

  # Backup Size (Ex. 1Gi, 100Mi)
  backupSize: "1Gi" 

  # Schedule period for the CronJob.
  # This spec allow you setup the backup frequency
  # Default: "0 0 * * *" # daily at 00:00
  schedule: "0 0 * * *"
EOF
```

Execute below command to create MariaDBBackup instance 

```execute
kubectl create -f MariaDBBackup.yaml -n my-mariadb-operator-app
```
Get the associated Pods:

```execute
kubectl get pods -n my-mariadb-operator-app
```


### Access MariaDB.
Execute the below commands to connect to the server and check for the newly created custom database (test-db)

```execute
kubectl exec -it mariadb-server-<podname> bash -n my-mariadb-operator-app
mysql -h <IP> -P 30685 -u db-user -pdb-user
show databases;

mysql -h <IP> -P 30685 -u root -ppassword
```