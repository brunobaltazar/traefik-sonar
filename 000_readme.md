Testado e validado
kubectl apply -f namespace.yaml  
kubectl apply -f volumes.yaml 
kubectl apply -f postgresql-deployment.yaml 
kubectl apply -f sonarqube-deployment.yaml
kubectl get all -n devops
kubectl delete -f namespace.yaml  -f volumes.yaml


kubectl port-forward $(kubectl get pods --selector "app.kubernetes.io/name=traefik" --output=name) 9000:9000

kubectl get service,pod -n devops
kubectl port-forward pod/sonarqube-f745bdbfc-sl997 port localhost:9000
kubectl port-forward sonarqube-f745bdbfc-sl997 9001:80
kubectl port-forward <pod-name> <locahost-port>:<pod-port>


Testado e validado
Todos os recursos do Helm são suportados. Por exemplo, instalando o gráfico em um namespace dedicado:


kubectl create ns traefik-devops
helm repo add traefik https://helm.traefik.io/traefik
helm repo update
helm install traefik traefik/traefik 
helm uninstall traefik traefik/traefik 
helm install traefik traefik/traefik 


kubectl apply -f dashboard.yaml
kubectl port-forward $(kubectl get pods --selector "app.kubernetes.io/name=traefik" --output=name) 9000:9000
Config etc host 127.0.0.1 traefik
acessa url http://traefik:9000/dashboard/#/
kubectl apply -f 001-app.yaml 
kubectl apply -f 002-app.yaml 
kubectl apply -f 003-app.yaml


Observação: o segredo e os certificados devem estar no mesmo namespace que o ingresso.
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.3.1/cert-manager.yaml 

https://github.com/jetstack/cert-manager/releases/download/v1.3.1/cert-manager.yaml
kubectl apply -f letsencrypt-issuer.yaml
kubectl get certificates  = resultado deve ser true 
kubectl describe certificates nginxapp.devopslabs.live  = resultado deve ser algo The certificate has been successfully issued
kubectl get secrets nginxapp.devopslabs.live.tls
















kubectl delete -f namespace.yaml  -f volumes.yaml




kubernetes.io/ingress.class: "nginx"
    certmanager.k8s.io/cluster-issuer: sonarqube
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/proxy-body-size: "20m"