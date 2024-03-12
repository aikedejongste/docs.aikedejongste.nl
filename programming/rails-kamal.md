---
layout: default
title: Rails - Kamal
has_children: false
parent: Programming
---

# Deploy Rails with Kamal

## Links

- [Traefik Dashboard](https://dev.to/tannakartikey/how-to-enable-traefik-dashboard-with-mrsk-48ef?comments_sort=latest)

## Docker credentials

If you get a Docker registry config with username and password in 1 you can separate them like this:

```bash
cat docker-config.json | jq -r '.auths | to_entries | .[0].key'
cat docker-config.json | jq -r '.auths | to_entries | .[0].value.auth' | base64 -d | cut -d ":" -f 1
cat docker-config.json | jq -r '.auths | to_entries | .[0].value.auth' | base64 -d | cut -d ":" -f 2
```


## Init Kamal

- bundle add kamal
- kamal init

Database.yaml
password: <%= ENV["POSTGRES_PASSWORD"] %>
host: <%= ENV["DB_HOST"] %>

10. assume ssl! not force


mkdir -p /opt/letsencrypt && touch /opt/letsencrypt/acme.json && chmod 600 /opt/letsencrypt/acme.json

docker network create -d bridge private


.env has priority over key files in config/credentials

but you need a master.key file?


