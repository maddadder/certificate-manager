https://np.reddit.com/r/kubernetes/comments/g3z5sp/microk8s_with_certmanager_and_letsecncrypt/
https://cert-manager.io/docs/concepts/
https://cert-manager.io/docs/tutorials/acme/ingress/#step-7-deploy-a-tls-ingress-resource

https://cert-manager.io/docs/tutorials/acme/ingress/
https://cert-manager.io/docs/faq/acme/


# Dashboard begin
microk8s enable dashboard
token=$(microk8s kubectl -n kube-system get secret | grep default-token | cut -d " " -f1)
microk8s kubectl -n kube-system describe secret $token
microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443
token:
see downloads 
# Dashboard end

microk8s enable helm3 ingress
microk8s kubectl create namespace cert-manager
microk8s helm3 repo add jetstack https://charts.jetstack.io
microk8s helm3 repo update
microk8s helm3 install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --set installCRDs=true \
  --set ingressShim.defaultIssuerName=letsencrypt-production \
  --set ingressShim.defaultIssuerKind=ClusterIssuer \
  --set ingressShim.defaultIssuerGroup=cert-manager.io

microk8s kubectl apply -f letsencrypt-staging-issuer.yaml
microk8s kubectl apply -f letsencrypt-production-issuer.yaml
microk8s kubectl describe issuer letsencrypt-staging
microk8s kubectl describe clusterissuer letsencrypt-production

microk8s kubectl apply -f ingress.yaml
microk8s kubectl get Issuers,ClusterIssuers,Certificates,CertificateRequests,Orders,Challenges --all-namespaces
microk8s kubectl get ingress
microk8s kubectl get secrets
microk8s kubectl describe certs
#micrk8s kubectl apply -f kuard.yaml
#microk8s kubectl apply letsencrypt-staging-issuer.yaml