---
layout: default
title: Postgres Analyze
parent: Databases
has_children: false
---

# Analyze Postgres

## 0. Vacuuming

[Article about vacuuming](https://www.datadoghq.com/blog/postgresql-vacuum-monitoring/)

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
