---
layout: default
title: Rails new app
has_children: false
parent: Programming
---

# New Ruby on Rails app

## New Rails app attempt number 2

1. Install Ruby
2. Install Rails gem (or check if you have the latest version)
3. rails new fwcheck --database=postgresql
4. apt install postgresql postgresql-contrib libpq-dev
5. sudo -u postgres createuser -s fwcheck -P
4. echo 'export APPNAME_DATABASE_PASSWORD="PostgreSQL_Role_Password"' >> ~/.bashrc
5. source ~/.bashrc
6. configure username and password for dev in config/database.yaml
  host: localhost
  username: me
  password: <%= ENV['APPNAME_DATABASE_PASSWORD'] %>
7.



## Kamal deploy

4. bundle add kamal
5. kamal init
10. assume ssl! not force

On target host:
mkdir -p /opt/letsencrypt && touch /opt/letsencrypt/acme.json && chmod 600 /opt/letsencrypt/acme.json
docker network create -d bridge private


