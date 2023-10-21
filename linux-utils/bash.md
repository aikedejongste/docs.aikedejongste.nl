---
layout: default
title: Bash
has_children: false
parent: Linux
---

# Bash

## Every bash script

* e -> exit on error
* u -> exit on unset variable referenced
* x -> show commands / activate debug out

```bash
#!/bin/bash
set -eu -o pipefail
set +x
```

## Find and delete old files

```bash
/usr/bin/find /var/backups/* -mtime +100 -name starts-with-* -type f -prune -exec rm -rf {} \;
```

## Find oldest directory (-d) or file (-f)

```bash
find /home/aike -type f -printf '%T+ %p\n' | sort | head -n 1
```

## Find by date

```bash
find . -name "*.yaml" -newermt "2023-09-22" ! -newermt "2023-09-23"
```


## Generate a password

```bash
tr -dc '[:alnum:]' </dev/urandom | head -c 12; echo
```

Add it to .bash_aliases:

```bash
alias genpw="tr -cd '[:alnum:]' < /dev/urandom | fold -w30 | head -n1"
```

## Add my public ssh key

```bash
mkdir -p .ssh && echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMJZhBxjBZgaU5JQWaS2smXC9IFS46jR5jVdDYHyq8DS" >> .ssh/authorized_keys && chmod 600 .ssh/authorized_keys
```

## Reset machine id

```bash
rm -f /etc/machine-id /var/lib/dbus/machine-id && dbus-uuidgen --ensure=/etc/machine-id && dbus-uuidgen --ensure
```

## Read input in a script

```bash
echo "Enter something:
read username
echo "$username"
```

## Bash for loop

```bash
for i in {1..5}; do COMMAND-HERE; done

or

for i in {1..5}; do COMMAND-HERE "$i"; done
for i in {1..5}; do echo "Hi, $i"; done

```

## Bash for loop over lines in a file

```bash
while read db; do  echo $db; done < /tmp/dbnames.txt
```

## Bash history

To immediately persist commands to your ~/.bash_history file and add a timestamp add this to ~/.bashrc:
More info: [here](https://www.cherryservers.com/blog/a-complete-guide-to-linux-bash-history)

```bash
HISTSIZE=11000
HISTFILESIZE=11000
HISTTIMEFORMAT="%F %T "
PROMPT_COMMAND='history -a'
```

## Convert heif/heic to jpg

```bash
apt-get install libheif-examples`
for file in *.heic; do heif-convert "$file" "heic/${file/%.heic/.jpg}"; done
```

## Cleanup Systemd journal

```bash journalctl --vacuum-time=1d```

## Yesterday

```bash
YESTERDAY=`date -d "yesterday 13:00" '+%Y-%m-%d'`

```

## SWAPPINESS

```bash
"echo 'vm.swappiness = 15' >> /etc/sysctl.conf"
```

## FIND INODE USAGE

```bash
find / -xdev -printf '%h\n' | sort | uniq -c | sort -k 1 -n
```

## SYSADMIN USER WITH SUDO

```bash
adduser sysadmin --disabled-password --ingroup sudo && echo 'sysadmin ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
```

## Email IP address with cron

```bash
25 20 * * *  /usr/bin/curl ifconfig.me | /usr/bin/mail -aFrom:hostname@you.nl -s "thuis ip" thuis@you.com
```

## Check for IO WAIT

```bash
iostat 1 10
or
for x in `seq 1 1 30`; do ps -eo state,pid,cmd | grep "^D"; echo "-"; sleep 2; done
```

## delete some file

```bash
shred -n 3 -z -u /root/github.txt
```

## Create a password

```bash
echo -n "Enter Password: " && head -1 </dev/stdin | tr -d '\n' | sha256sum | cut -d" " -f1
```

## Crazy way to set file permissions

```bash
#!/bin/sh
while f=$(inotifywait -e create --format "%f" /var/www/company/htdocs/ ) ; do
        chmod 664 '/var/www/company/htdocs/'$f
done
```
