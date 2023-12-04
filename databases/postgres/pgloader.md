---
layout: default
title: Postgres - pgloader
parent: Databases
---

# pgloader

## Notes

none yet.

## Example config MySQL to Postgres

```
load database
     from      mysql://root:asdfasdfasdfasfdasdf47fba@1.2.3.4/db-name
     into      postgresql:///import21jan25

CAST type datetime to timestamp
               drop default drop not null using zero-dates-to-null,
     type date drop not null drop default using zero-dates-to-null

ALTER SCHEMA 'blaat' RENAME TO 'public'

INCLUDING ONLY TABLE NAMES MATCHING 'users', 'relations', 'addresses', 'contacts'

ALTER TABLE NAMES MATCHING 'table-name-x'   RENAME TO 'old-table-name-x'

BEFORE LOAD DO
$$ create schema if not exists public; $$;

```

## Example config MSSQL to Postgres

```
load database
     from mssql://SA:password@mssql2:1433/db-name
     into postgresql://postgres:root@localhost:5432/db-name

set work_mem to '128MB', maintenance_work_mem to '1024 MB'

ALTER SCHEMA 'dbo' RENAME TO 'public'

before load do $$ drop schema if exists dbo cascade;
 $$;
```
