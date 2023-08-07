---
layout: default
title: Logrotate
parent: Logging
---

# Logrotate

## Commands

* A dry run: `logrotate -vd /etc/logrotate.d/app-name`
* An actual run: `logrotate -v /etc/logrotate.d/app-name`
* An forced run: `logrotate -vf /etc/logrotate.d/app-name`

## Comments

* Logrotate does not check if there is enough diskspace

`su sysadmin sysadmin` is required when ...

## Example config for a Rails app

```bash
/home/sysadmin/checkout/log/production.log {
  daily
  missingok
  rotate 5
  compress
  copytruncate
  delaycompress
  notifempty
  su sysadmin sysadmin
  create 666 sysadmin sysadmin
  sharedscripts
}
```
