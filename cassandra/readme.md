# on daffy
```
microk8s add-node
```
# go to cassandra:
```
sudo snap install microk8s --classic --channel=latest/edge
sudo usermod -a -G microk8s alice
sudo chown -f -R alice ~/.kube
newgrp microk8s
```
- paste in contents from the --worker flag add node

kubectl apply -k github.com/k8ssandra/cass-operator/config/deployments/default?ref=v1.10.1

kubectl apply -f storage-class.yaml
kubectl -n cass-operator apply -f dc1.yaml
