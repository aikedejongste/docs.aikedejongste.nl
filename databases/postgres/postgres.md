---
layout: default
title: Postgres
parent: Databases
has_children: true
---

# Postgres

## Backup options

* [PG-Backup-local-container](https://github.com/prodrigestivill/docker-postgres-backup-local) - have not tried it yet


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

