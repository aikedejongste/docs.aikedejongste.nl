---
layout: default
title: Postgres - Timescaledb
parent: Databases
---

# Timescaledb


## Notes

- Extensions are per database, not per server
- The user who runs CREATE EXTENSION becomes the owner of the extension for purposes of later privilege checks, and normally also becomes the owner of any objects created by the extension's script.
- Only superusers can create the extension or update it to a newer version.

## Installation

```bash
sudo add-apt-repository ppa:timescale/timescaledb-ppa
sudo apt install timescaledb-postgresql-13
```

OR

```bash
docker pull timescale/timescaledb-ha:pg14-latest
```

Enable with `CREATE EXTENSION IF NOT EXISTS timescaledb;`

## Show installed version

```bash
SELECT default_version, installed_version FROM pg_available_extensions where name = 'timescaledb';
```

## Troubleshooting

`ERROR:  could not open extension control file "/usr/share/postgresql/13/extension/timescaledb.control":
No such file or directory`

## See version in use

`SELECT * FROM pg_available_extensions WHERE name = 'timescaledb_toolkit';`
`SELECT * FROM pg_available_extensions WHERE name = 'timescaledb';`
