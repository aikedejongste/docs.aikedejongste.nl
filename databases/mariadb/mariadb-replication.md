---
layout: default
title: MariaDB Replication
has_children: false
parent: Databases
---

# Mariadb replication

## docker-compose primary

## docker-compose secondary

Replace the IPs!

```yaml
version: '3'

services:
  mariadb-slave:
    image: docker.io/bitnami/mariadb:10.7
    container_name: mariadb-slave
    restart: always
    ports:
      - '0.0.0.0:3306:3306'
    volumes:
      - '/opt/mariadb_slave_data:/bitnami/mariadb'
    environment:
      - MARIADB_ROOT_PASSWORD=12345
      - MARIADB_MASTER_HOST=1.1.1.1
      - MARIADB_MASTER_ROOT_USER=repl_user
      - MARIADB_MASTER_ROOT_PASSWORD=123456
      - MARIADB_REPLICATION_MODE=slave
      - MARIADB_REPLICATION_USER=repl_user
      - MARIADB_REPLICATION_PASSWORD=2345678
      - ALLOW_EMPTY_PASSWORD=no
      - MARIADB_EXTRA_FLAGS=--expire_logs_days=7 --skip-name-resolve
      - MARIADB_SKIP_TEST_DB=yes
      - MARIADB_USER=root
      - MARIADB_PASSWORD=123456789
    healthcheck:
      test: ['CMD', '/opt/bitnami/scripts/mariadb/healthcheck.sh']
      interval: 15s
      timeout: 5s
      retries: 6
  mysqld-exporter:
    image: quay.io/prometheus/mysqld-exporter
    command:
     - --collect.info_schema.tablestats
    container_name: mysqld-exporter
    environment:
      - DATA_SOURCE_NAME=root:12345678@(mariadb-slave:3306)/
    ports:
      - '0.0.0.0:9104:9104'
    links:
      - mariadb-slave
    depends_on:
      - mariadb-slave
```
