# How to setup cert-manager in microk8s

1. Install microk8s 
```
sudo snap install microk8s --classic --channel=1.28
```

2. Grant access to kubectl
```
sudo usermod -a -G microk8s $USER
sudo chown -f -R $USER ~/.kube
su - $USER
```

3. optionally enable remote access
```
sudo apt-get install ssh
cd .kube
microk8s kubectl config view --raw > config
# then copy the .kube folder to your remote machine
```

4. enable dashboard
```
microk8s enable dashboard
microk8s kubectl describe secret -n kube-system microk8s-dashboard-token
kubectl port-forward -n kube-system service/kubernetes-dashboard 11443:443
```

5. enable cert-manager
```
microk8s enable cert-manager
```
6. Follow the instructions in either `istio-ingress` or the `nginx-ingress` folder
7. Install ingress
```
microk8s enable ingress
```
8. Install load balancer, and specify the ip range of the load balancer
```
microk8s enable metallb
```
