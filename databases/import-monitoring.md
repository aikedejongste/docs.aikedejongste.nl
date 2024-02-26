---
layout: default
title: Import monitoring
has_children: false
parent: Databases
---

# Import monitoring

This is a pretty rough script to check if data is still current. It was used to
check if a BI-import was still current.


```bash
#!/bin/bash

set -e
set -o pipefail

# Credentials for management
export PGHOST=
export PGPORT=5432
export PGUSER=
export PGSSLMODE=prefer
export PGPASSWORD='123'
export PGDATABASE=company_bi
export SCHEMA_NAME="public"
export MONITOR_URL='https://cronitor.link/p/..../....'

QUERY_RESULT=$(psql -t -c "select created_at from user_sessions order by ID DESC LIMIT 1;")

# Extract the date and time from the result
DATE_TIME=$(echo $QUERY_RESULT | awk '{print $1 " " $2}')

# Convert the date-time string to seconds since 1970
SECONDS_SINCE=$(date -d "$DATE_TIME" '+%s')

# Get current time in seconds since 1970
CURRENT_SECONDS=$(date '+%s')

# Calculate the difference in seconds
DIFF_SECONDS=$((CURRENT_SECONDS - SECONDS_SINCE))
echo "Difference: $DIFF_SECONDS"

# Check if the difference is more than 1 day (86400 seconds in a day)
if [[ $DIFF_SECONDS -gt 161800 ]]; then
    echo "The database has not been in use for more than 1 day."
    curl $MONITOR_URL?state=fail
else
    echo "The database is still in use."
    curl $MONITOR_URL?state=complete
fi
```
