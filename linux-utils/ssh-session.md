---
layout: default
title: SSH Sessions with AUTH SOCK
has_children: false
parent: Linux
---

# SSH Sessions with AUTH SOCk

In `~/.ssh/rc`

```
if test "$SSH_AUTH_SOCK"; then
        ln -sf $SSH_AUTH_SOCK ~/.ssh/ssh_auth_sock
fi
```

In `~/.bashrc`

```
export SSH_AUTH_SOCK=~/.ssh/ssh_auth_sock
```
