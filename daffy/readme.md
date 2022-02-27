# daffy
```
sudo snap install microk8s --classic --channel=latest/edge
sudo usermod -a -G microk8s alice
sudo chown -f -R alice ~/.kube
newgrp microk8s
microk8s enable dashboard
microk8s enable metallb
```
# add a single ip of the master node that will be exposed to the internet
```
token=$(microk8s kubectl -n kube-system get secret | grep default-token | cut -d " " -f1)
microk8s kubectl -n kube-system describe secret $token
microk8s.kubectl config view --raw
```
# on client
# copy text from above and paste into ~/.kube/config on client machine (changing the ip to the master node) and install kubectl
```
kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443 --address 0.0.0.0
```

# back on daffy
```
microk8s add-node
```

# go to dizzy:
```
sudo snap install microk8s --classic --channel=latest/edge
sudo usermod -a -G microk8s alice
sudo chown -f -R alice ~/.kube
newgrp microk8s
```
- paste in contents from the --worker flag add node

# back on daffy
```
microk8s enable istio
microk8s enable registry
```

# on client
- install docker https://docs.docker.com/engine/install/ubuntu/
```
sudo nano /etc/docker/daemon.json
{
  "insecure-registries" : ["192.168.1.151:32000"]
}
sudo systemctl restart docker
```
- see https://microk8s.io/docs/registry-private for error: http: server gave HTTP response to HTTPS client

sudo snap install helm --classic

# on daffy and all other nodes
```
nano /var/snap/microk8s/current/args/containerd.toml
sudo mkdir -p /var/snap/microk8s/current/args/certs.d/192.168.1.151:32000
sudo touch /var/snap/microk8s/current/args/certs.d/192.168.1.151:32000/hosts.toml
sudo nano /var/snap/microk8s/current/args/certs.d/192.168.1.151:32000/hosts.toml
server = "http://192.168.1.151:32000"

[host."192.168.1.151:32000"]
capabilities = ["pull", "resolve"]


microk8s kubectl create namespace cert-manager
microk8s enable helm3
microk8s kubectl create namespace cert-manager
microk8s helm3 repo add jetstack https://charts.jetstack.io
microk8s helm3 repo update
microk8s helm3 install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --set installCRDs=true \
  --set ingressShim.defaultIssuerName=letsencrypt-production \
  --set ingressShim.defaultIssuerKind=Issuer \
  --set ingressShim.defaultIssuerGroup=cert-manager.io

microk8s kubectl create secret generic -n cert-manager route53-secret --from-literal=secret-access-key="YOUR_KEY_FROM_AWS"
microk8s kubectl create secret generic -n istio-system route53-secret --from-literal=secret-access-key="YOUR_KEY_FROM_AWS"
```

# on client
```
kubectl apply -f letsencrypt-production-clusterissuer.yaml
kubectl apply -f istio-certificates.yaml
kubectl apply -f service-mesh.yaml
kubectl apply -f leenet-gateway.yaml
```
- cert progress
```
kubectl get Issuers,ClusterIssuers,Certificates,CertificateRequests,Orders,Challenges --all-namespaces

```