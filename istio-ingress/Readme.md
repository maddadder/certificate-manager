1. Deploy secrets
```
kubectl create namespace istio-system
kubectl create secret generic -n cert-manager route53-secret --from-literal=secret-access-key="YOUR_KEY_FROM_AWS"
kubectl create secret generic -n istio-system route53-secret --from-literal=secret-access-key="YOUR_KEY_FROM_AWS"
```
2. Deploy cluster issuer, certificates, gateway, virtual services, etc
```
helm install istio-gateway istio-ingress/istio-gateway --namespace istio-system
```
3. check the status of the certificate deployments to/from lets encrypt
```
kubectl get Issuers,ClusterIssuers,Certificates,CertificateRequests,Orders,Challenges --all-namespaces
```
