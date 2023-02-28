---
layout: default
title: Bash
has_children: false
parent: Linux
---

# Bash

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

## Reset machine id:
rm -f /etc/machine-id /var/lib/dbus/machine-id && dbus-uuidgen --ensure=/etc/machine-id && dbus-uuidgen --ensure


## Read input in a script:

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

## Bash history

To immediately persist commands to your ~/.bash_history file and add a timestamp add this to ~/.bashrc:
More info: [here](https://www.cherryservers.com/blog/a-complete-guide-to-linux-bash-history)

```bash
HISTSIZE=11000
HISTFILESIZE=11000
HISTTIMEFORMAT="%F %T "
PROMPT_COMMAND='history -a'
```





