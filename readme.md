### Clona projeto
```shell
 git clone git@github.com:brunobaltazar/traefik-sonar.git
```

```shell
 cd traefik-sonar
```

### Cria namespace no cluster

```shell
kubectl apply -f apps/namespace.yaml
```


### Instalação cert-manager para o cluster

> Observação: o segredo e os certificados devem estar no mesmo namespace que o ingresso.

```shell
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.3.1/cert-manager.yaml 
```

```shell
kubectl get all --namespace cert-manager
```

```shell
kubectl apply -f letsencrypt/prod-letsencrypt-issuer.yaml
kubectl apply -f letsencrypt/staging-letsencrypt-issuer.yaml
```
> Resultado de get ClusterIssuer deve ser "True"

```shell
kubectl get ClusterIssuer

NAME                        READY    AGE
letsencrypt-prod-devops      True    21s
letsencrypt-staging-devops   True    7s 
```


### Instalação Traefik

```shell
kubectl apply -f traefik/traefik_lb.yaml
```

```shell
kubectl get svc -n devops
```
> Adicionar o IP do LoadBalancer no seu registro de DNS e criar as entradas de subdominio.

```shell
traefik    LoadBalancer   10.0.7.176    20.206.224.41   80:31505/TCP,8080:31547/TCP,443:30950/TCP   28m
```

```shell
kubectl apply -f apps/app_01.yaml
kubectl apply -f apps/app_02.yaml
kubectl apply -f apps/app_03.yaml
```

```shell
kubectl get certificates -n one 
kubectl get certificates -n two 
kubectl get certificates -n devops 
```

> Após a criação dos subdominios pode levar alguns minutos para o cert-manager realizar a criação dos certificados auto-assinados ou validos.
```shell
NAME                            READY   SECRET                         AGE
one.devopslabs.live-tls         True    one.devopslabs.live-tls        26m

NAME                            READY   SECRET                         AGE
nginxapp.devopslabs.live.tls    True    nginxapp.devopslabs.live.tls   26m

NAME                            READY   SECRET                         AGE
two.devopslabs.live.tls         True    two.devopslabs.live.tls        28m
```

```shell
kubectl get certificates -n one
```

```shell
kubectl describe certificates nginxapp.devopslabs.live -n one 
```

> resultado deve ser algo The certificate has been successfully issued

```shell
kubectl get secrets nginxapp.devopslabs.live.tls -n one
```


















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
