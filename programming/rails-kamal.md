---
layout: default
title: Rails Kamal
has_children: false
parent: Programming
---

# Deploy Rails with Kamal

## Links

- [Traefik Dashboard](https://dev.to/tannakartikey/how-to-enable-traefik-dashboard-with-mrsk-48ef?comments_sort=latest)

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


