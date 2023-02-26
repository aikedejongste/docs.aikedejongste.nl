---
layout: default
title: Linux
has_children: true
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

