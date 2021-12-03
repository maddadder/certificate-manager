https://voyagermesh.com/docs/v2021.10.18/guides/cert-manager/get-started/
microk8s kubectl create namespace cert-manager
microk8s kubectl label namespace cert-manager certmanager.k8s.io/disable-validation=true
microk8s kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.4.1/cert-manager.yaml

https://voyagermesh.com/docs/v2021.10.18/guides/cert-manager/dns01_challenge/aws-route53/

microk8s kubectl create secret generic route53-secret --from-literal=secret-access-key="YOUR_KEY_FROM_AWS"
microk8s kubectl delete -f letsencrypt-staging-issuer.yaml
microk8s kubectl apply -f letsencrypt-staging-clusterissuer.yaml
#microk8s kubectl delete -f letsencrypt-production-issuer.yaml
microk8s kubectl apply -f kuard.yaml
microk8s kubectl apply -f certificate.yaml
#microk8s kubectl delete -f production-certificate.yaml
microk8s kubectl apply -f ingress.yaml
microk8s kubectl get certificate.cert-manager.io --all-namespaces
microk8s kubectl describe certificate.cert-manager.io leenet-route53-dns


# Troubleshooting
404 on ingress: https://stackoverflow.com/questions/54506269/simple-ingress-from-host-with-microk8s

certificate not valid: https://cert-manager.io/docs/faq/troubleshooting/
certificate not valid: https://cert-manager.io/docs/faq/acme/

microk8s kubectl get all --all-namespaces

microk8s kubectl get Issuers,ClusterIssuers,Certificates,CertificateRequests,Orders,Challenges

microk8s kubectl get orders