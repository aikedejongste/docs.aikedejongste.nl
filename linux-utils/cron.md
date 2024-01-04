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
* * * * * myjob.sh >> /var/log/myjob_`date +\%Y\%m\%d_\%H\%M`.log 2>&1
```

## Choose editor for Crontab

```bash
select-editor
```

## Cron config location

```
/var/spool/cron/crontabs/
```

## Cron logs location

...

## Cron logs to separate log

Enable cron.log in `/etc/rsyslog.d/50-default.conf` and restart rsyslog.

## To use docker-compose exec in cron use -T

To avoid this error: `"The input device is not a TTY".`

```bash
/usr/bin/docker-compose --file /opt/sentry/docker-compose.yml exec -T worker sentry cleanup --days 30 >> /var/log/cron.log 2>&1
```
