---
layout: default
title: MongoDB
parent: Databases
has_children: false
---

# MongoDB

## Connect to a server

Install mongosh first, it is a separate package.

```bash
mongosh --host 1.2.3.4
```

## Connect to database

`use db-name`

## Show collections (tables)

`.....`

## Show databases

`show dbs`

## Show users

```bash
test> use admin
switched to db admin

admin> show collections;
system.users
system.version

admin> db.system.users.find()
```

## Show running config

```bash
db.runCommand({ getCmdLineOpts: 1 })

or

db.getCmdLineOpts()
```

