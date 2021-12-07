# Istio begin
microk8s enable istio
microk8s kubectl apply -f leenet-gateway.yaml
microk8s enable metallb
microk8s kubectl create secret generic -n istio-system route53-secret --from-literal=secret-access-key="YOUR_KEY_FROM_AWS"
microk8s kubectl apply -f letsencrypt-staging-issuer.yaml
microk8s kubectl apply -f istio-certificate.yaml
