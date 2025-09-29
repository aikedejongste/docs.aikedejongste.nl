---
layout: default
title: MariaDB - Connect
has_children: false
parent: Databases
---

# Mariadb - How to connect

## Install only the client

```bash
apt install mariadb-client
```

## Bash console

```bash
export MYSQL_HOST='your_mysql_host'
export MYSQL_USER='your_username'
export MYSQL_PASSWORD='your_password'
export MYSQL_DB='your_database'

mysql
```

## Defaults file

```yaml
[client]
user=foo
password=P@55w0rd
```
