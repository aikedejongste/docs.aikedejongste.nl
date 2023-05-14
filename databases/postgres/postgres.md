---
layout: default
title: Postgres
parent: Databases
has_children: true
---

# Postgres

## Backup options

* [PG-Backup-local-container](https://github.com/prodrigestivill/docker-postgres-backup-local) - have not tried it yet

## Create database if not exists
```
select 'create database mydb' WHERE NOT EXISTS (SELECT FROM pg_database WHERE datname = 'mydb')\gexec
```

## Install only the client to test a connection

```bash apt install postgresql-client-14```


## Connect with the client to a remote host

```bash psql -h <REMOTE HOST> -p <REMOTE PORT> -U <DB_USER> <DB_NAME> -W```

Non interactive password:

```bash PGPASSWORD=postgresSuperUserPsw psql -h localhost -U postgres```


## Config

SELECT pg_read_file('pg_hba.conf');
table pg_hba_file_rules;


SELECT pg_reload_conf();


## Users and roles

In PostgreSQL you can create a new user using the CREATE USER or the CREATE ROLE command. The difference between these two options is that CREATE USER sets the LOGIN privilege directly while CREATE ROLE will set this attribute to NOLOGIN.

