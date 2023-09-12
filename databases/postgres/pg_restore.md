---
layout: default
title: Postgres - pg_restore
parent: Databases
---

# pg_restore

## psql vs pg_restore

User psql for plain sql dumps, pg_restore for other formats.

Custom and directory format dumps offer a lot of advantages over plain
SQL script dumps, and I use them exclusively. You can selectively
restore only some tables/schema, can choose whether to include only
schema, only data, or both at restore time, etc.

Lots of the options you have to specify at pg_dump time with
SQL-format dumps can be now chosen at restore-time if you use a
custom-format dump and pg_restore.

## Things that don't work

pg_restore doesn't provide a built-in option to rename the database
during restoration. So you'll need to follow a two-step
process: drop and create. Or only use --clean and don't add --create.

## Options you always need

* `-d` - target database (can also be set with PGDATBASE env var)
* `-U` - user/role (can also be set with PGUSER env var)
* `-Fd / -Fc` - directory format or compressed single file

## Good options for restoring backups

* `--disable-triggers` - disable triggers

## Good options for migrations/bi/development

* `--clean` - Used to drop database objects before recreating them.
* `--create` - Used to create a database before restoring it.
* `--no-privileges` - Prevent restoration of access privileges (ACLs aka grant/revoke commands).
* `--exclude-schema` - Prevent restoration of a schema
* `--no-owner` - makes whatever you put after -U the new owner
* `-j4` - speed things up if using directory format (4 = number cores)

## Example restore script

```bash
#!/bin/bash
export PGHOST=host.name.com
export PGPORT=5432
export PGUSER="username"
export PGSSLMODE=prefer
export PGPASSWORD='1234567890'
export PGDATABASE=my_company_db

# Create extensions if required
psql -c "CREATE EXTENSION IF NOT EXISTS plpgsql WITH SCHEMA pg_catalog;"

# Restore without caring about permissions and the original data:
pg_restore --clean --no-privileges --no-owner -Fc -d $PGDATABASE < prod.dump

# Restore and map all ownership and permissions to a new role and delete old data:
# pg_restore --clean --role=$PGUSER -Fc -d $PGDATABASE < prod.dump
```

## Optional part to restore permissions for readonly user

```bash
# Restore permissions for readonly user
psql -U $PGUSER <<EOF
-- Connect to the target database
\c $PGDATABASE

-- Revoke all existing privileges for this user
REVOKE ALL ON ALL TABLES IN SCHEMA $SCHEMA_NAME FROM $READONLY_USERNAME;
REVOKE ALL ON ALL SEQUENCES IN SCHEMA $SCHEMA_NAME FROM $READONLY_USERNAME;

-- Grant usage permission on the schema
GRANT USAGE ON SCHEMA $SCHEMA_NAME TO $READONLY_USERNAME;

-- Grant select permission on all current tables
GRANT SELECT ON ALL TABLES IN SCHEMA $SCHEMA_NAME TO $READONLY_USERNAME;

-- Grant select permission on future tables
ALTER DEFAULT PRIVILEGES IN SCHEMA $SCHEMA_NAME GRANT SELECT ON TABLES TO $READONLY_USERNAME;
EOF

echo "Read-only permissions reapplied for user $READONLY_USERNAME on database $PGDATABASE"
```

