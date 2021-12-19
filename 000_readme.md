======================================================================================================================
Testado e validado
kubectl apply -f namespace.yaml  
kubectl apply -f volumes.yaml 
kubectl apply -f postgresql-deployment.yaml 
kubectl apply -f sonarqube-deployment.yaml
kubectl get all -n devops
kubectl delete -f namespace.yaml  -f volumes.yaml
======================================================================================================================



======================================================================================================================
Testado e validado
Todos os recursos do Helm são suportados. Por exemplo, instalando o gráfico em um namespace dedicado:
kubectl create ns traefik-devops
helm repo add traefik https://helm.traefik.io/traefik
helm repo update
helm install traefik traefik/traefik 
helm uninstall traefik traefik/traefik 
helm install traefik traefik/traefik 

======================================================================================================================

======================================================================================================================
kubectl apply -f dashboard.yaml
kubectl port-forward $(kubectl get pods --selector "app.kubernetes.io/name=traefik" --output=name) 9000:9000
Config etc host 127.0.0.1 traefik
acessa url http://traefik:9000/dashboard/#/
kubectl apply -f 001-app.yaml 
kubectl apply -f 002-app.yaml 
kubectl apply -f 003-app.yaml

======================================================================================================================



kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.3.1/cert-manager.yaml

kubectl delete -f namespace.yaml  -f volumes.yaml  -f postgresql-deployment.yaml -f sonarqube-deployment.yaml



kubernetes.io/ingress.class: "nginx"
    certmanager.k8s.io/cluster-issuer: sonarqube
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/proxy-body-size: "20m"