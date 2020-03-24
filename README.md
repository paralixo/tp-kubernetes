# TP Kubernetes
Par nous

## Etapes
```
$ minikube start
$ kubectl apply -f .\namespaces\namespace1.yml
$ helm repo add appscode https://charts.appscode.com/stable/
$ helm repo update
$ helm install kubedb-operator appscode/kubedb --version v0.13.0-rc.0 --namespace kube-system
# Vérifier installation (kubedb-operator est censé être présent)
$ kubectl describe deployments --namespace kube-system
```

## Découpage de namespace
3 namespaces:
- Base de donnée
- Wordpress
- Monitoring

## KubeDB
### Mise en palce de KubeDB
```
$ helm repo add appscode https://charts.appscode.com/stable/
$ helm repo update
$ helm install kubedb-operator appscode/kubedb --version v0.13.0-rc.0 --namespace kube-system
```
### Comprendre le principe de CRD (custom resource definition)
TODO avec MySQL pour WP
TODO faire kubeDB avant deployment wordpress
## Wordpress
TODO https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/
## RBAC
## Monitoring
## OIDC
## Registry + GUI Web
