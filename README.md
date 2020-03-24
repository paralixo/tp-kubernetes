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
### Proposer un découpage
3 namespaces:
- Base de donnée
- Wordpress
- Monitoring
### Créer les namespaces avec leurs quotas
Requirements source: 
- https://www.servermania.com/kb/articles/what-are-the-requirements-for-a-wordpress-server/
- https://dev.mysql.com/doc/mysql-monitor/4.0/en/system-prereqs-reference.html
- https://grafana.com/docs/grafana/latest/installation/requirements/
- https://prometheus.io/docs/prometheus/1.8/storage/#memory-usage
TODO: regarder les recommendations de quotas de machine (et le noter dans le readme)
```
$ kubectl apply -f ./configs/base-de-donnees.yml
$ kubectl apply -f ./configs/wordpress.yml
$ kubectl apply -f ./configs/monitoring.yml
```

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
