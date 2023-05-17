---
layout: default
title: Postgres - pg_restore
parent: Databases
---

# pg_restore

## psql vs pg_restore

User psql for plain sql dumps, pg_restore for other formats.

Custom and directory format dumps offer a lot of advantages over plain SQL script dumps, and I use them exclusively. You can selectively restore only some tables/schema, can choose whether to include only schema, only data, or both at restore time, etc. 

Lots of the options you have to specify at pg_dump time with SQL-format dumps can be now chosen at restore-time if you use a custom-format dump and pg_restore.


## Options you always need

* `-d` - target database (can also be set with PGDATBASE env var)
* `-U` - user/role (can also be set with PGUSER env var)
* `-Fd / -Fc` - directory format or compressed single file


## Good options for backups


## Good options for migrations/bi/development


* `--clean` - Used to drop database objects before recreating them.
* `--create` - Used to create a database before restoring it.
* `--no-privileges` - Prevent restoration of access privileges (ACLs aka grant/revoke commands).
* `--exclude-schema` - Prevent restoration of a schema
* `--no-owner` - makes whatever you put after -U the new owner
* `-j4` - speed things up if using directory format (4 = number cores)



