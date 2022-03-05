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
- paste in contents from the --worker flag add node

# on the client
```
kubectl apply -k github.com/k8ssandra/cass-operator/config/deployments/default?ref=v1.10.1

kubectl apply -f storage-class.yaml
kubectl -n cass-operator apply -f dc1.yaml
```
# on cassanda
```
cd /var/snap/microk8s/common/default-storage
sudo chmod 777 *
```