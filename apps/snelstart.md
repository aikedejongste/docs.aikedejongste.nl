---
layout: default
title: Snelstart
has_children: false
parent: Self Hosted Apps
---

# Snelstart

## Restore a database backup and check it

### Something about permissions

```chmod 777 /test```

### Start a MS SQL server container

TODO: add docker-compose.yaml here

### Exec into container

```docker exec -it -u 0 snelstartbu_mssql_1 /bin/bash```

### Connect/import the database

```/opt/mssql-tools/bin/sqlcmd -S . -U sa -P 12345678 -Q "CREATE DATABASE [POS] ON (FILENAME ='/test.mdf'), (FILENAME = '/test.ldf') FOR ATTACH"```

### Run a query to check for latest invoice

```/opt/mssql-tools/bin/sqlcmd -S . -U sa -P 12345678 -Q "select top 5 * from POS.dbo.FactuurNummer ORDER BY fldFactuurNummerID DESC"```

### Show schemas

```select s.name as schema_name, s.schema_id, u.name as schema_owner from sys.schemas s inner join sys.sysusers u on u.uid = s.principal_id order by s.name```

### Show tables

```SELECT * FROM POS.information_schema.tables WHERE TABLE_TYPE='BASE TABLE' AND TABLE_SCHEMA = 'dbo'```
