---
layout: default
title: Pageduty
parent: Various scripts
has_children: false
---

# Pagerduty

```bash
#!/bin/bash

# setup
source ~/.pdtoken
KEY=`tr -dc '[:alnum:]' </dev/urandom | head -c 12; echo`

# check
echo "Token is $PDTOKEN"
echo "Service is $SERVICE"
echo "Key is $KEY"

# run
curl --request POST \
  --url https://api.pagerduty.com/incidents \
  --header 'Accept: application/vnd.pagerduty+json;version=2' \
  --header "Authorization: Token token=$PDTOKEN" \
  --header 'Content-Type: application/json' \
  --header 'From: developers@company.com' \
  --data '{
  "incident": {
    "type": "incident",
    "title": "A deployment has failed.",
    "service": {
      "id": "'"$SERVICE"'",
      "type": "service_reference"
    },
    "urgency": "low",
    "incident_key": "'"$KEY"'",
    "body": {
      "type": "incident_body",
      "details": "Please investigate because the code and the db schema may not be compatible."
    }
  }
}'
```
