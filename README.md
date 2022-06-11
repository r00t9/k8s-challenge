# How to install it

## Install elasticsearch with helm

```
helm repo add elastic https://helm.elastic.co
helm install elasticsearch elastic/elasticsearch --create-namespace -n elk -f ./values.yaml
```
## Install kibana with helm
```
helm install kibana elastic/kibana -n elk -f ./values.yaml
```
## Install mysql
```
kubectl create namespace database
kubectl create secret generic -n database wordpress-mysql-secret --from-literal=MYSQL_ROOT_PASSWORD=root_password --from-literal=MYSQL_DATABASE=wp01 --from-literal=MYSQL_USER=root
```
```
kubectl apply -f yaml/mysql/multi_resource.yaml
```
## create user 
```
create database wp02;
CREATE USER 'wp01'@'%' IDENTIFIED BY 'pass';
GRANT ALL PRIVILEGES ON wp01.* TO 'wp01'@'%' WITH GRANT OPTION;
CREATE USER 'wp02'@'%' IDENTIFIED BY 'pass';
GRANT ALL PRIVILEGES ON wp02.* TO 'wp02'@'%' WITH GRANT OPTION;
```
## Install wordpress 

### Create namespace and secret 
```
kubectl create namespace client-a
kubectl create namespace client-b
kubectl create secret generic -n client-a wp01-mysql-secret --from-literal=WORDPRESS_DB_PASSWORD=pass
kubectl create secret generic -n client-b wp02-mysql-secret --from-literal=WORDPRESS_DB_PASSWORD=pass
```

### Install Wordpress
```
kubectl apply -f yaml/wp01/multi_resource.yaml
kubectl apply -f yaml/wp02/multi_resource.yaml
```
## Install nginx ingress
```
kubectl apply -f yaml/nginx.yaml
```