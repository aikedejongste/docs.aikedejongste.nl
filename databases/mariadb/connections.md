---
layout: default
title: MariaDB - Connections
parent: Databases
---

# Connections

### Show current connections

```show status where variable_name = 'threads_connected';```

### Show max connections

```SHOW VARIABLES LIKE 'max_connections';```
