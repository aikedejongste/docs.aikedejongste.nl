---
layout: default
title: Nodejs
has_children: false
parent: Programming
---

# Nodejs

## Ubuntu installation

```bash
export NODE_MAJOR=20
apt-get update &&  apt-get install -y ca-certificates curl gnupg
mkdir -p /etc/apt/keyrings && curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key |  gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" |  tee /etc/apt/sources.list.d/nodesource.list
apt-get update &&  apt-get install nodejs -y
```

## Yarn installation

```bash
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg |  apt-key add -

echo "deb https://dl.yarnpkg.com/debian/ stable main" |  tee /etc/apt/sources.list.d/yarn.list

apt update && apt install yarn
```

