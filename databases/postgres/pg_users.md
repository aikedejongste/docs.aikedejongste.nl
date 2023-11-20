---
layout: default
title: Postgres - create users
parent: Databases
---

# Postgres Users and Roles

Usernames with underscores are preferred over usernames with dashes.

## Users and roles

In PostgreSQL you can create a new user using the CREATE USER or the CREATE ROLE command.
The difference between these two options is that CREATE USER sets the LOGIN privilege
directly while CREATE ROLE will set this attribute to NOLOGIN.


## Delete a user

`DROP USER IF EXISTS readonly_user`

## Script that creates one or more read-only users + schema permissions

TODO: script should remove user and recreate if password is set

```bash
#!/bin/bash

# Credentials for management
export PGPASSWORD=
export PGSSLMODE=prefer
export PGUSER=postgres
export PGHOST=
export PGPORT=5432

export PGDATABASE=

# Credentials for read-only user
#READONLY_PASSWORD=`tr -dc '[:alnum:]' </dev/urandom | head -c 12; echo`
READONLY_USERNAMES=("grafana" "grafana2")
READONLY_PASSWORDS=("1234" "4567")
# Add your schemas in a space-separated list here or leave empty for all
#SCHEMAS=("public my_schema")

for i in "${!READONLY_USERNAMES[@]}"; do
  USERNAME=${READONLY_USERNAMES[$i]}
  PASSWORD=${READONLY_PASSWORDS[$i]}

  # If SCHEMAS is empty, fetch all schema names, otherwise use the specified list
  SCHEMA_CONDITION=""
  if [ -z "$SCHEMAS" ]; then
    echo "Using all schemas."
    SCHEMA_CONDITION="SELECT schemata.schema_name FROM information_schema.schemata"
  else
    SCHEMA_CONDITION="SELECT unnest(string_to_array('$SCHEMAS', ' '))"
  fi


  psql -v ON_ERROR_STOP=1 <<EOF

    -- Create the read-only user with login privileges
    DO
    \$\$BEGIN
      CREATE USER $USERNAME WITH PASSWORD '$PASSWORD' LOGIN;
    EXCEPTION WHEN DUPLICATE_OBJECT THEN
      RAISE NOTICE 'User $USERNAME already exists. Skipping creation.';
    END\$\$;

    -- Revoke all existing privileges for this user
    REVOKE ALL PRIVILEGES ON DATABASE $PGDATABASE FROM $USERNAME;

    -- Grant connect permission to the database
    GRANT CONNECT ON DATABASE $PGDATABASE TO $USERNAME;

    -- Connect to the target database
    \c $PGDATABASE

    -- Grant or revoke privileges on specified or all schemas
    DO
    \$$
    DECLARE
        schema_name text;
    BEGIN
        FOR schema_name IN ($SCHEMA_CONDITION)
        LOOP
            -- Execute a test query, e.g., SELECT 1; You can replace the following line with any test query
            EXECUTE 'SELECT 1;';

            -- Simple test query
            RAISE NOTICE 'Testing access for schema: %', schema_name;

            -- Revoke existing privileges
            EXECUTE 'REVOKE ALL PRIVILEGES ON ALL TABLES IN SCHEMA ' || schema_name || ' FROM ' || '$USERNAME';
            EXECUTE 'REVOKE ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA ' || schema_name || ' FROM ' || '$USERNAME';

            --  -- Grant new privileges
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
