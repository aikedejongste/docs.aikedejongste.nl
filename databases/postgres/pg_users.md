---
layout: default
title: Postgres - users
parent: Databases
---

# Postgres Users and Roles

## Delete a user

`DROP USER IF EXISTS readonly_user`

## Example script that creates a read-only user

It has read permissions on 1 schema.

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
REVOKE ALL ON DATABASE $PGDATABASE FROM $READONLY_USERNAME;
REVOKE ALL ON ALL TABLES IN SCHEMA $SCHEMA_NAME FROM $READONLY_USERNAME;
REVOKE ALL ON ALL SEQUENCES IN SCHEMA $SCHEMA_NAME FROM $READONLY_USERNAME;

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

## Read-only user for all schemas

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

-- Grant connect permission to the database
GRANT CONNECT ON DATABASE $PGDATABASE TO $READONLY_USERNAME;

-- Revoke all existing privileges for this user
DO
\$\$
DECLARE
    schema_name text;
BEGIN
    FOR schema_name IN (SELECT schemata.schema_name FROM information_schema.schemata)
    LOOP
        EXECUTE 'REVOKE ALL ON ALL TABLES IN SCHEMA ' || schema_name || ' FROM $READONLY_USERNAME';
        EXECUTE 'REVOKE ALL ON ALL SEQUENCES IN SCHEMA ' || schema_name || ' FROM $READONLY_USERNAME';
    END LOOP;
END;
\$\$;

-- Grant usage and select permission on all schemas
DO
\$\$
DECLARE
    schema_name text;
BEGIN
    FOR schema_name IN (SELECT schemata.schema_name FROM information_schema.schemata)
    LOOP
        EXECUTE 'GRANT USAGE ON SCHEMA ' || schema_name || ' TO $READONLY_USERNAME';
        EXECUTE 'GRANT SELECT ON ALL TABLES IN SCHEMA ' || schema_name || ' TO $READONLY_USERNAME';
        EXECUTE 'ALTER DEFAULT PRIVILEGES IN SCHEMA ' || schema_name || ' GRANT SELECT ON TABLES TO $READONLY_USERNAME';
    END LOOP;
END;
\$\$;
EOF

echo "Read-only user $READONLY_USERNAME created for database $PGDATABASE with password $READONLY_PASSWORD."
```

## Create multiple read-only users

```bash
#!/bin/bash

export PGDATABASE=bi-database
export SCHEMA_NAME="public"

READONLY_USERNAMES=("one" "two" "three")
READONLY_PASSWORDS=("1234" "4567" "89101")

for i in "${!READONLY_USERNAMES[@]}"; do
      USERNAME=${READONLY_USERNAMES[$i]}
      PASSWORD=${READONLY_PASSWORDS[$i]}

  psql <<EOF
    -- Create the read-only user with login privileges
    CREATE USER $USERNAME WITH PASSWORD '$PASSWORD' LOGIN;

    -- Connect to the target database
    \c $PGDATABASE

    -- Revoke all existing privileges for this user
    REVOKE ALL ON DATABASE $PGDATABASE FROM $USERNAME;
    REVOKE ALL ON ALL TABLES IN SCHEMA $SCHEMA_NAME FROM $USERNAME;
    REVOKE ALL ON ALL SEQUENCES IN SCHEMA $SCHEMA_NAME FROM $USERNAME;

    -- Grant connect permission to the database
    GRANT CONNECT ON DATABASE $PGDATABASE TO $USERNAME;

    -- Revoke all existing privileges for this user
    DO
    \$$
    DECLARE
        schema_name text;
    BEGIN
        FOR schema_name IN (SELECT schemata.schema_name FROM information_schema.schemata)
        LOOP
            EXECUTE 'REVOKE ALL ON ALL TABLES IN SCHEMA ' || schema_name || ' FROM ' || '$USERNAME';
            EXECUTE 'REVOKE ALL ON ALL SEQUENCES IN SCHEMA ' || schema_name || ' FROM ' || '$USERNAME';
        END LOOP;
    END;
    \$$;

    -- Grant usage and select permission on all schemas
    DO
    \$$
    DECLARE
        schema_name text;
    BEGIN
        FOR schema_name IN (SELECT schemata.schema_name FROM information_schema.schemata)
        LOOP
            EXECUTE 'GRANT USAGE ON SCHEMA ' || schema_name || ' TO ' || '$USERNAME';
            EXECUTE 'GRANT SELECT ON ALL TABLES IN SCHEMA ' || schema_name || ' TO ' || '$USERNAME';
            EXECUTE 'ALTER DEFAULT PRIVILEGES IN SCHEMA ' || schema_name || ' GRANT SELECT ON TABLES TO ' || '$USERNAME';
        END LOOP;
    END;
    \$$;
EOF

    echo "Read-only user $USERNAME created for database $PGDATABASE with password $PASSWORD."
done
```
