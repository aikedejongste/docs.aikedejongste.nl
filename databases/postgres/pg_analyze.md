---
layout: default
title: Postgres Analyze
parent: Databases
has_children: false
---

# Analyze Postgres

## 0. Vacuuming

[Article about vacuuming](https://www.datadoghq.com/blog/postgresql-vacuum-monitoring/)

Check if `autovacuum` is on:

```sql
SELECT name, setting FROM pg_settings WHERE name='autovacuum';
```

## 1. Show long running queries

```sql
SELECT pid,
       usename,
       query,
       state,
       query_start,
       now() - query_start AS duration
FROM pg_stat_activity
WHERE state = 'active'
ORDER BY duration DESC
LIMIT 10;
```

## 2. Fake a slow query

```sql
SELECT pg_sleep(10);

SELECT pg_sleep(100); -- triggered by Aike
```

## 3. Show last VACUUM time

```sql
SELECT relname, last_vacuum, last_autovacuum FROM pg_stat_user_tables;
```

## 4. Show tables with dead rows

```sql
SELECT relname, n_dead_tup FROM pg_stat_user_tables;
```

## 5. Find table with highest diskspace usage

```sql
SELECT
       relname AS "table_name",
       pg_size_pretty(pg_table_size(C.oid)) AS "table_size"
FROM
       pg_class C
LEFT JOIN pg_namespace N ON (N.oid = C.relnamespace)
WHERE nspname NOT IN ('pg_catalog', 'information_schema') AND nspname !~ '^pg_toast' AND relkind IN ('r')
ORDER BY pg_table_size(C.oid)
DESC LIMIT 1;
```

## 6. Estimate effect of VACUUM on diskspace

TODO: test this.

```sql
CREATE EXTENSION pgstattuple;
SELECT pgstattuple('your_table_name');
```

Interpreting the Results:

The table_len field shows the total size of the table.
The dead_tuple_len field shows the size of the dead tuples.
The free_space field shows the amount of free space already available in the table.
The space that can potentially be reclaimed by a VACUUM is approximately the sum of dead_tuple_len and free_space.
