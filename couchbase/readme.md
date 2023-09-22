### on daffy
```
# https://github.com/couchbase-partners/helm-charts
microk8s helm3 repo add couchbase https://couchbase-partners.github.io/helm-charts/
microk8s helm3 repo update
# copy over operator-values.yaml to home dir
microk8s helm3 install couchbase-operator --values operator-values.yaml couchbase/couchbase-operator
```
### on client
```
# https://docs.couchbase.com/operator/current/howto-couchbase-create.html
kubectl apply -f couchbase-default-secret.yaml
# NOTE: when you change the password make sure it matches the secret and visa-versa. You should let the operator set the password by updating this secret after it sets the default password

# when applying the couchbase-cluster.yaml below, note the nodeselector
# on the backup:. This should match where the pvc mount to
# when the pvc mounts. You should see it in dashboard: 
# persistent volumns > my-backup > 
# label: hostPathProvisionerIdentity: labelname (that matches the node
# that the pv is attached to)
kubectl apply -f couchbase-cluster.yaml
kubectl apply -f couchbase-bucket.yaml
kubectl apply -f destination-rule.yaml
```

### configure backup
```
kubectl apply -f couchbase-backup.yaml
```

### to restore from backup
```
#note: This will restore from the latest backup once applied. You probably need to flush the bucket before this will work
kubectl apply -f couchbase-restore.yaml
```