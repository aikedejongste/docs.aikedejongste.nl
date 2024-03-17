---
layout: default
title: MariaDB - Binlogs
parent: Databases
---

# Binlogs

## Show binlogs

`SHOW BINARY LOGS'`

## Show binlogs related config

`show variables like '%bin%';`

## Show retention time

`show variables like "expire_logs_days";`

## Set retention time

`SET GLOBAL expire_logs_days = 7;`

## Stream binlogs

```bash
mysqlbinlog --read-from-remote-source=BINLOG-DUMP-GTIDS --user=replication --host=10.x.x.x --password=********* --start-position=123 --stop-never --to-last-log mysql-bin.000183 | mysql -h 192.168.1.1 --binary-mode -u admin -p --max_allowed_packet=512M
```
