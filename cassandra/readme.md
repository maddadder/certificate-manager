# on daffy
```
microk8s add-node
microk8s enable openebs
```
# go to cassandra:
```
sudo snap install microk8s --classic --channel=latest/edge
sudo usermod -a -G microk8s alice
sudo chown -f -R alice ~/.kube
newgrp microk8s
```
- ^ paste in contents from the --worker flag from the `microk8s add-node command` on daffy

# on the client
```
kubectl create -f namespace.yaml

# kubectl apply -k github.com/k8ssandra/cass-operator/config/deployments/default?ref=v1.10.1
OR
# If you wish to install it with cluster wide rights to monitor all the namespaces for CassandraDatacenter objects, use the following command:
kubectl apply -k github.com/k8ssandra/cass-operator/config/deployments/cluster?ref=v1.10.1
# You need the /cluster (vs /default) version (do not use the /default version if you want to apply to other namespaces)

kubectl apply -f storage-class.yaml

kubectl -n cassdb apply -f dc3.11.7.yaml
OR 
kubectl -n cassdb apply -f dc4.0.1.yaml
```
# on cassanda node
```
cd /var/snap/microk8s/common/default-storage
sudo chmod 777 *
```
- ^ You have to do the above because we are using the microk8s.io/hostpath storage class
# troubleshooting
```
the namespace from the provided object "cass-operator" does not match the namespace "cassdb". You must pass '--namespace=cass-operator' to perform this operation.
```
- ^ Happens when you apply the operator outside the cass-operator namespace