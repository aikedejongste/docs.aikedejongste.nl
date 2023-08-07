---
layout: default
title: Postgres - users
parent: Databases
---

# Postgres Users and Roles

## Delete a user

`DROP USER IF EXISTS readonly_user`

## Example script that creates a read-only user

```bash
#!/bin/bash

# Credentials for management
export PGHOST=
export PGPORT=5432
export PGUSER="you"
export PGSSLMODE=prefer
export PGPASSWORD='....'
export PGDATABASE=jajaja
export SCHEMA_NAME="public"

# Credentials for read-only user
READONLY_USERNAME="readonly_user"
READONLY_PASSWORD=`tr -dc '[:alnum:]' </dev/urandom | head -c 12; echo`

psql <<EOF
-- Create the read-only user with login privileges
CREATE USER $READONLY_USERNAME WITH PASSWORD '$READONLY_PASSWORD' LOGIN;

-- Connect to the target database
\c $PG_DATABASE

-- Revoke all existing privileges for this user
-- REVOKE ALL ON DATABASE $PGDATABASE FROM $READONLY_USERNAME;
-- REVOKE ALL ON ALL TABLES IN SCHEMA $SCHEMA_NAME FROM $READONLY_USERNAME;
-- REVOKE ALL ON ALL SEQUENCES IN SCHEMA $SCHEMA_NAME FROM $READONLY_USERNAME;

-- Grant connect permission to the database
GRANT CONNECT ON DATABASE $PGDATABASE TO $READONLY_USERNAME;

-- Grant usage permission on the schema
GRANT USAGE ON SCHEMA $SCHEMA_NAME TO $READONLY_USERNAME;

-- Grant select permission on all current tables
GRANT SELECT ON ALL TABLES IN SCHEMA $SCHEMA_NAME TO $READONLY_USERNAME;

-- Grant select permission on future tables
ALTER DEFAULT PRIVILEGES IN SCHEMA $SCHEMA_NAME GRANT SELECT ON TABLES TO $READONLY_USERNAME;
EOF

echo "Read-only user $READONLY_USERNAME created for database $PGDATABASE with password $READONLY_PASSWORD."

```

