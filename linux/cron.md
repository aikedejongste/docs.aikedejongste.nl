---
layout: default
title: CRON
has_children: false
parent: Linux
---

# CRON

## Log cron output and errors to file
```
* * * * * myjob.sh >> /var/log/myjob.log 2>&1
```

## Choose editor for Crontab
```bash
select-editor
```
