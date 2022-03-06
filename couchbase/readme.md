# on daffy
```
# https://github.com/couchbase-partners/helm-charts
microk8s helm3 repo add couchbase https://couchbase-partners.github.io/helm-charts/
microk8s helm3 repo update
# copy over values.yaml to home dir
microk8s helm3 install couchbase-operator --values operator-values.yaml couchbase/couchbase-operator
```
# on client
```
# https://docs.couchbase.com/operator/current/howto-couchbase-create.html
kubectl apply -f couchbase-cluster.yaml
kubectl apply -f destination-rule.yaml
```
