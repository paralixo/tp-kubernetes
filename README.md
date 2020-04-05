# TP Kubernetes
Par Verner BOISSON, Antoine-Dominique (STEFANAGGI?) et Florian LAFUENTE

## Etapes d'installation
```
# Génération de la VM
$ minikube start

# Installation de KubeDB
$ helm repo add appscode https://charts.appscode.com/stable/
$ helm repo update
$ helm install kubedb-operator appscode/kubedb --namespace kube-system
$ helm install kubedb-catalog appscode/kubedb-catalog --version 0.12.0 --namespace kube-system

# Vérifier installation de KubeDB (kubedb-operator est censé être présent)
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

## KubeDB
### Mise en palce de MySQL avec KubeDB

Nous allons installer une base MySQL à l'aide de KubeDB (précédemment installé). Pour visualiser notre base nous allons aussi mettre en place
un service phpMyAdmin :

```
# Mise en place de la configuration
$ kubectl.exe apply -f ./conf/database/namespace.yml
namespace/db created
$ kubectl.exe apply -f ./conf/database/myadmin.yml
deployment.apps/myadmin created
service/myadmin created
$ kubectl.exe apply -f ./conf/database/mysql.yml
mysql.kubedb.com/mysql created

# On vérifie que les pods de phpMyAdmin et MySQL sont présents
$ kubectl.exe get pods -n db
NAME                       READY   STATUS    RESTARTS   AGE
myadmin-58cd6c758c-bkf68   1/1     Running   0          46s
mysql-0                    1/1     Running   0          37s
```

Pour faire la liaison entre phpMyAdmin et MySQL il nous faut les identifiants de connexion et l'IP de MySQL

``` 
$ kubectl get pods mysql-0 -n demo -o yaml -n db | grep podIP
  podIP: 172.17.0.7
  podIPs:
$ kubectl get secrets -n db mysql-auth -o jsonpath='{.data.\username}' | base64 -d
root
$ kubectl get secrets -n db mysql-auth -o jsonpath='{.data.\password}' | base64 -d
_YFjm8AlqL7omjyq
```

On se connecte maintenant sur phpMyAdmin :
```
$ minikube service myadmin -n db --url
http://192.168.99.102:30569
```

Cependant nous avons une erreur lors de la connexion :

![Erreur sur phpmyadmin](./images/phpmyadmin-not-allowed.png)

Nous avons un problème pour lancer des commandes sur notre pod mysql (le pod n'est pas en erreur) :

```
# Probleme pour joindre mysql qui bloque la suite 
$ kubectl.exe exec mysql-0 -it -n db -- mysql -u root --password=_YFjm8AlqL7omjyq
error: unable to upgrade connection: container not found ("mysql")
```

### Comprendre le principe de CRD (custom resource definition)
TODO

## Wordpress
TODO https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/
### Mettre en place un wordpress répliqué
### Utilisation des ConfigMap et/ou Secret
### Un service redondant
### Backups de la base de données

## RBAC

## Monitoring

## OIDC

## Registry + GUI Web
