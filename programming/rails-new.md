---
layout: default
title: Rails - New app
has_children: false
parent: Programming
---

# New Ruby on Rails app

## New Rails app attempt number 2

1. Install Ruby
2. Install Rails gem (or check if you have the latest version)
3. rails new fwcheck --database=postgresql
4. apt install postgresql postgresql-contrib libpq-dev

5. Postgres in Docker:

set username and password in docker-compose

run 'export DATABASE_URL=postgres://user:pass@127.0.0.1/'

version: '3.7'

services:
  db:
    image: postgres:15
    restart: always
    ports:
      - 0.0.0.0:5432:5432
    environment:
      POSTGRES_PASSWORD: example
    volumes:
      - ./_PGDATA:/var/lib/postgresql/data

5. sudo -u postgres createuser -s fwcheck -P

OR

- `CREATE USER aike WITH PASSWORD '123';`
- `ALTER ROLE aike CREATEROLE SUPERUSER CREATEDB;`

4. echo 'export APPNAME_DATABASE_PASSWORD="PostgreSQL_Role_Password"' >> ~/.bashrc
5. source ~/.bashrc
6. configure username and password for dev in config/database.yaml
  host: localhost
  username: me
  password: <%= ENV['APPNAME_DATABASE_PASSWORD'] %>
7. [Install Devise](https://docs.aikedejongste.nl/programming/rails-devise.html)
8. bundle add administrate

- rails g scaffold Address user:references ip:string name:string
- rails g scaffold Result address:references port:integer status:string scanned_at:datetime protocol:string tls:boolean

## Kamal deploy

4. bundle add kamal
5. kamal init
10. assume ssl! not force

On target host:
mkdir -p /opt/letsencrypt && touch /opt/letsencrypt/acme.json && chmod 600 /opt/letsencrypt/acme.json
docker network create -d bridge private
