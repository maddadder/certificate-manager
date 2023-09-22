# How to setup cert-manager in microk8s


1. Deploy secrets
```
kubectl create namespace nginx-system
kubectl create secret generic -n cert-manager route53-secret --from-literal=secret-access-key="YOUR_KEY_FROM_AWS"
kubectl create secret generic -n nginx-system route53-secret --from-literal=secret-access-key="YOUR_KEY_FROM_AWS"
```
2. Deploy cluster issuer, certificates, gateway, virtual services, etc
```
helm install nginx-gateway nginx-ingress/nginx-gateway --namespace nginx-system
```
3. check the status of the certificate deployments to/from lets encrypt
```
kubectl get Issuers,ClusterIssuers,Certificates,CertificateRequests,Orders,Challenges --all-namespaces
```
