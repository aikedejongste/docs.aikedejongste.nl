---
layout: default
title: SSH Session logging
has_children: false
parent: Linux
---

# SSH Session logging

## Option 1: scriptreplay

Via [this page](https://aws.amazon.com/blogs/security/how-to-record-ssh-sessions-established-through-a-bastion-host/).

```bash
LOG_FILE="`date --date="today" "+%Y-%m-%d_%H-%M-%S"`_`whoami`"
LOG_DIR="/tmp/"

# Print a welcome message
echo ""
echo "NOTE: This SSH session will be recorded"
echo "AUDIT KEY: $LOG_FILE"
echo ""

# Wrap an interactive shell into "script" to record the SSH session
script -qf --timing=$LOG_DIR$LOG_FILE.time $LOG_DIR$LOG_FILE.data --command=/bin/bash
```

Force it when a user logs in with:

```
echo -e "\nForceCommand /usr/bin/ssh-logger" >> /etc/ssh/sshd_config

```

in the sshd_config. And then use this to replay:

```bash
scriptreplay --timing=2023-05-17_08-17-44_aike.time 2023-05-17_08-17-44_aike.data
```

## Option 2: sudosh

Relevant link: [Github](https://github.com/cloudposse/sudosh)
This claims to be a continuation of the project: [sudosh2](https://github.com/squash/sudosh2)
https://unix.stackexchange.com/questions/198936/how-to-log-all-things-that-happened-via-an-ssh-session


Defaults log_output
Defaults!/usr/bin/sudoreplay !log_output
Defaults!/sbin/reboot !log_output


## Option 3: containerSSH

Website [here](https://containerssh.io/), but unfortunately there is very little activity going on in the project at the moment.
