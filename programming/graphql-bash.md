---
layout: default
title: GraphQL
has_children: false
parent: Programming
---

# GraphQL with bash

## Get the access token

```bash
USER=a@b.nl
PASS=123
AT=$(curl -d "grant_type=password&client_id=cloud-tilaa&username=$USER&password=$PASS" -X POST https://auth.tilaa.com/auth/realms/Tilaa/protocol/openid-connect/token |& jq .access_token)
export AT=$AT
echo $AT
```

## Get the basic structure of the API

```bash
# assuming $AT is the access_token
curl -g \
-X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $AT" \
-d '{"query": "query { __schema { types { name kind description fields(includeDeprecated: true) { name description type { name kind } args { name description type { name kind } defaultValue } } } } }"}' \
https://graphql.tilaa.com/
```
