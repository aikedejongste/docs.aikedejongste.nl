---
layout: default
title: pg_restore
parent: Postgres
grand_parent: Databases
---

# pg_restore

You can exclude a schema when restoring with `-N schema-name`

```
pg_restore -d myappdb -N schema_name myappdb.dump
 OR
pg_restore -d myappdb --exclude-schema=schema_name myappdb.dump
```

Create the database when restoring with 


