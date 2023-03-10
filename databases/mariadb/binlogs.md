---
layout: default
title: Binlogs
parent: Databases
---

# Binlogs

## Show binlogs:
`SHOW BINARY LOGS'`

## Show binlogs related config:
`show variables like '%bin%';`

## Show retention time:
`show variables like "expire_logs_days";`

## Set retention time:
`SET GLOBAL expire_logs_days = 7;`
