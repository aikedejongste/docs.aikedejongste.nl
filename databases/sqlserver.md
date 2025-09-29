---
layout: default
title: SQL Server
parent: Databases
has_children: false
---

# SQL Server

- You have to type `GO` after each command to execute it.

## Login vs User

SQL Login is for Authentication and SQL Server User is for Authorization.
Login is created at the SQL Server instance level and User is created at the SQL Server database level.

## Create login

```
USE master;
CREATE LOGIN [testkees] WITH PASSWORD = '123456789L';

or

CREATE LOGIN [testkees] WITH PASSWORD = '123456789L', DEFAULT_DATABASE=[appnamedata];
```

## Create user

```
USE appnamedata;
CREATE USER [testkees] FOR LOGIN [MOSOS1\testkees];
```

## Use Windows login??

```
use master;
CREATE LOGIN [MOSOS1\testkees] FROM WINDOWS;

USE appnamedata;
GO
CREATE USER [MOSOS1\testkees];
GO
```

## Grant a role

```
USE appnamedata;
GO
ALTER ROLE [db_datareader] ADD MEMBER [MOSOS1\testkees];
GO
```

## Show current user

```
SELECT CURRENT_USER;
GO
```

## Some query with information

```
SELECT
  *
FROM
  databaseName.INFORMATION_SCHEMA.TABLES;
GO
````
