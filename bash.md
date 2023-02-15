---
layout: default
title: Bash
has_children: false
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
mkdir -p .ssh && echo "ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAsFMSthnrlRdXwvQMgDNZXXn9e3sfivmN7YcDtZAGnNHsfwxmfCzTfEO95Ee9dZTkyRODLYl/ivIUBLpkmi8QP9B8haSedZuNHc4yXM75iaCkel5yC1Xc4+Czq9yLTdFyJxB/kAKpjfkZKh5LmOm+7NbK1AjRM2KdobkhWG+BTCPP3c8EX7ztOLLzs23iIL85w5eKvXFjYftfnaDskUFiORU+W58KlcA+2TVc+v0MmdZ1pLmNZuaXFrHcSBj3/mk30vuvjsH5KTy5bGCwHIGID2xB2b5JaE2Wc7dEF/L4Z0Ybkty3mlkx0/raixrS1nGZN10kxZu72I+J+RgZhChAow== aike@boos" >> .ssh/authorized_keys && chmod 600 .ssh/authorized_keys
```

## Reset machine id:
rm -f /etc/machine-id /var/lib/dbus/machine-id && dbus-uuidgen --ensure=/etc/machine-id && dbus-uuidgen --ensure



