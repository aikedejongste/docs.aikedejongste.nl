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

## Create database in MariaDB in a Pod

```bash
kubectl exec my-mariadb-0 -n my-mariadb-ns -- sh -c 'mysql -uroot -p$MARIADB_ROOT_PASSWORD -e "create database dbname"'
```

## Show grants

```
show grants for 'username'@'10.0.0.%';
```

## Kill Slow queries

```
SELECT GROUP_CONCAT(CONCAT('KILL ',id,';') SEPARATOR ' ') '
```

## Paste the following query to kill all processes

```
' FROM information_schema.processlist WHERE time>60\G
```

## Create read-only user with IP lock

```
GRANT LOCK TABLES, SELECT ON userbase.* TO 'grafana'@'10......' IDENTIFIED BY '12345678';
```

## Create read-only user without IP lock

```
GRANT LOCK TABLES, SELECT ON plaza.* TO 'grafana'@'%' IDENTIFIED BY '123456789';
```

## Create normal user

```
mysql -e "CREATE USER IF NOT EXISTS $DB_USER@APP_HOST IDENTIFIED BY '123456789';"
```

## Delete a user

```
drop user 'mysqld_exporter'@'localhost';
```

## List users

```
select user, host from mysql.user;
```

## Script for adding users

```bash
# dit werkt niet met 10.0.0.% lijkt het
# use some backtics for database names with a dash
APP_HOST=10.0.0.X
DB_NAME=appname
DB_USER=<some-user>
DB_PASS=<some-password>
mysqladmin --default-character-set=utf8mb4 create $DB_NAME
mysql -e "CREATE USER IF NOT EXISTS $DB_USER@APP_HOST IDENTIFIED BY '$DB_PASS';"
mysql -e "GRANT LOCK TABLES,ALTER,CREATE,DELETE,DROP,INDEX,INSERT,SELECT,UPDATE ON $DB_NAME.* TO $DB_NAME@\"$APP_HOST\" IDENTIFIED BY '$DB_PASS'; FLUSH PRIVILEGES;"
```
