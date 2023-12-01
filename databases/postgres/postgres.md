---
layout: default
title: Postgres
parent: Databases
has_children: false
---

# Postgres

## 1. Show databases

`\l`

## 2. Connect to database

`\c`

## 3. Show schemas

`\dn`

## 4. List extensions

Extensions are per database. Not for the whole server.

`\dx`

## 5. Show tables

`\dt or \dt+`

## Show all tables in all schemas

`\dt+ *.*`

## List roles (users)

`\du`

## Check access for role

`SELECT table_catalog, table_schema, table_name, privilege_type FROM information_schema.table_privileges WHERE  grantee = '< YOU >';`

## Check if role can access a view

## Change user password

You can use double quotes for the role name but this is not required. You
should use single quotes for the password. This is because in SQL syntax,
double quotes are used to quote identifiers (such as table names, column names,
and role names), while single quotes are used to quote string literals
(such as values and passwords).

`ALTER USER "role_name" WITH PASSWORD 'new_password';`

and this should do the same:

`ALTER ROLE role_name WITH PASSWORD 'new_password';`

## Rename user or role

`ALTER ROLE old_role_name RENAME TO new_role_name;`

## Disable or enable login for role

`psql -t -c "ALTER ROLE $READONLY_USERNAME WITH NOLOGIN;"`

`psql -t -c "ALTER ROLE $READONLY_USERNAME WITH LOGIN;"`

## Backup options

* [PG-Backup-local-container](https://github.com/prodrigestivill/docker-postgres-backup-local) - have not tried it yet

## Create database if not exists

```sql
select 'create database mydb' WHERE NOT EXISTS (SELECT FROM pg_database WHERE datname = 'mydb')\gexec
```

## Install only the client to test a connection

```bash
apt install postgresql-client-15
```

## Connect with the client to a remote host

```bash
psql -h <REMOTE HOST> -p <REMOTE PORT> -U <DB_USER> <DB_NAME> -W
```

Non interactive password:

```bash
PGPASSWORD=postgresSuperUserPsw psql -h localhost -U postgres
```

## Show active connections

```bash
SELECT * FROM pg_stat_activity;
```

## Config

SELECT pg_read_file('pg_hba.conf');
table pg_hba_file_rules;

SELECT pg_reload_conf();

## Execute sql from a file

```bash
psql -U $DB_USER -d $DB_NAME -a -f $SQL_FILE
```

The -a option in the psql command prints all input lines to the standard output,
so you'll see the SQL statements as they're executed. The -f option specifies the file to execute.

## Create schema

To execute the CREATE SCHEMA statement, you must have the CREATE privilege in the current database.

```bash
CREATE SCHEMA IF NOT EXISTS mlops;
```
TODO: SET OWNER
