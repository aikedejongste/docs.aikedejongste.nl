---
layout: default
title: MariaDB - Backups
parent: Databases
---

# MariaDB Backups

## Backup script with health check for all dbs

### Put MySQL credentials in ~/.my.cnf like this

```
[client]
user = read-only-backup-user
password = .....
host = 192.168.1.3
```

### The backup script

```bash
#!/bin/bash

set -o errexit
set -o nounset
set -o pipefail

echo -e "Sending start signal to HealhChecks.io\n"
curl -s https://hc-ping.com/fd9340f1-8a0f-448e.........../start

DEST="/var/backups/mysql"
DATE=`date +%u`

for DB in $(mysql --execute 'show databases' -s --skip-column-names | grep -v information_schema | grep -v performance_schema | grep -v meta | grep -v sys | grep -v mysql); do
  mysqldump --no-tablespaces --single-transaction $DB | gzip > "$DEST/$DB.$DATE.sql.gz";
done

echo -e "Sending SUCCESS signal to HealhChecks.io\n"
curl -s https://hc-ping.com/fd9340f1-8a0f-448e..........
```

## Useful options for mysqldump

- `--no-tablespaces` - don't include tablespace information in the dump
- `--single-transaction` - dump consistent snapshot without locking tables
- `--routines` - include stored routines
- `--triggers` - include triggers
- `--events` - include events
- `--comments` - include comments in the dump
- `--dump-date` - include dump date in comments
- `--hex-blob` - dump binary columns in hexadecimal format
- `--skip-lock-tables` - don't lock tables for read
- `--opt` - shorthand for `--add-drop-table --add-locks --create-options --disable-keys --extended-insert --lock-tables --quick --set-charset`
