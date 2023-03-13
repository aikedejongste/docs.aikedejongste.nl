---
layout: default
title: MariaDB
has_children: false
parent: Databases
---

# Mariadb

## Access MariaDB console when installed with Bitnami Helm Chart

```
mysql -uroot -p$MARIADB_ROOT_PASSWORD
```

Or with Kubectl

```
kubectl exec --stdin --tty mariadb-pod-0 -n mariadb-namepsace -- /bin/bash -c '/opt/bitnami/mariadb/bin/mysql -uroot -p$MARIADB_ROOT_PASSWORD'
```

## Create database in MariaDB in a Pod:

```bash
kubectl exec my-mariadb-0 -n my-mariadb-ns -- sh -c 'mysql -uroot -p$MARIADB_ROOT_PASSWORD -e "create database dbname"'
```

## Show grants

```
show grants for 'username'@'10.0.0.%';
```

## Docker-compose example

```yaml
version: '3.7'

services:
  app:
    image: your-app
    container_name: app
    restart: always
    environment:
      MARIADB_ROOT_PASSWORD: example
  db:
    image: mariadb
    container_name: db
    restart: always
    environment:
      MARIADB_ROOT_PASSWORD: example
  pma:
    image: phpmyadmin/phpmyadmin
    links:
      - db
    environment:
      PMA_HOST: db
      PMA_PORT: 3306
      PMA_ARBITRARY: 1
    restart: always
    ports:
      - 8081:80
  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
```
