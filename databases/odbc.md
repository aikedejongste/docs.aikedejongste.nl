---
layout: default
title: ODBC
#nav_order: 50
has_children: true
parent: Databases
---

# ODBC

## Related Ubuntu packages

- odbc-mariadb - ODBC driver for MariaDB
- unixodbc - middleware

UnixODBC is the middleware that helps an application to communicate
with any DBMS using ODBC, while ODBC-MariaDB is a specific driver
that facilitates communication between applications and MariaDB
or MySQL databases via the ODBC layer.

## Files and locations

- /etc/odbc.ini -> credentials
- /etc/odbcinst.ini -> driver config

## Verify

```bash
root@0a52919b3f97:/# odbcinst -j
unixODBC 2.3.9
DRIVERS............: /etc/odbcinst.ini
SYSTEM DATA SOURCES: /etc/odbc.ini
FILE DATA SOURCES..: /etc/ODBCDataSources
USER DATA SOURCES..: /root/.odbc.ini
SQLULEN Size.......: 8
SQLLEN Size........: 8
SQLSETPOSIROW Size.: 8
```
