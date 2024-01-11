---
layout: default
title: Postgres - pg_dump
#parent: Postgres
parent: Databases
---

# pg_dump

## Related

- [pghoard backups](https://github.com/Aiven-Open/pghoard)
- [script from Postgres itself](https://wiki.postgresql.org/wiki/Automated_Backup_on_Linux)
- [PGbackrest](https://pgbackrest.org/)
- [postgres-backup-s3 container](https://github.com/eeshugerman/postgres-backup-s3)

## Step by step

## Need pg_dump?

It is part of this package:

```bash
apt install postgresql-client-14
```

## WITH roles (good for backups)

## WITHOUT roles (good for migrations)

- `--no-privileges`
- `--no-owner`

## Options and examples

- `-Fp` = format plain, this is fastest but produces normal SQL.
- `-Fc` = format custom, much smaller result, much slower. Suitable for input into pg_restore.
- `-Fd` = format directory, this is required for parallel jobs.
- `--file=NAME OF DUMP` = The file/directory to output to.
- `-N 'plpsql'` = exclude an extension
- `--jobs=NUMBER OF CORES` = This allows the dump to run multiple jobs.
- `--data-only` = This is the flag to dump only the data from the tables and not the schema information.
- `--schema=public` = Dump to only the public schemas, for most cases this is all you need.
- `--table=NAME OF TABLE` = This flag specifies which table to dump. Multiple --table flag may be used together.

## Other option

```bash
$ pg_dump -Z0 -j 10 -Fd database_name -f dumpdir
$ tar -cf - dumpdir | pigz > dumpdir.tar.gz
$ rm dumpdir
```

## Quick dump and restore
```bash
pg_dump -Fc -Z 9  --file=file.dump myDb
pg_restore -Fc -j 8  file.dump
```

## Performance and convenience

### Compression

The formats -Fd and -Fc have the same compression, but -Fd is 3 times faster to dump.

```pg_dump dbname --format=custom --no-privileges --disable-triggers -Fd -j4 -U username -f data.pgdump```

takes 7 minutes

```pg_dump dbname --format=custom --no-privileges --disable-triggers -Fp -U username | pigz > data.pigz```

takes 11 minutes

The disk usage is the same. Directory format is hard to transfer.

## Example script

```bash
#!/bin/bash
PGUSER=
PGPASSWORD=
PGSSLMODE=
PGHOST=
PGPORT=
PGDATABASE=
MONITOR_URL=
DESTINATION=
DATE=`date +%d`
LIST=$(psql -l | grep UTF8 | awk '{ print $1}' | grep -vE '^-|^List|^Name|template[0|1]')

set -e
set -x

for d in $LIST
do
  pg_dump -Z1 -Fc $d -f $DESTINATION/daily.$DATE.$d.dump || exit 1
  pg_dump --format=custom --no-privileges --disable-triggers -Fp | pigz > data.pigz
done

curl -s $MONITOR_URL
```
