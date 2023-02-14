---
layout: default
title: MariaDB
#nav_order: 50
has_children: true
#permalink: /docs/ui-component
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
