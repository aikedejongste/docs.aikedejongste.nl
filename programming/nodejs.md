---
layout: default
title: Nodejs
has_children: false
parent: Programming
---

# Nodejs

## Install with NVM

So it is easier to choose the version you want.

### Install the latest LTS version

```bash
nvm install --lts
```

### Install a specific version

```bash
nvm install 18.17.0
```

### Install the latest current release

```bash
nvm install node
```

### Use a specific version

```bash
nvm use 18.17.0
```

### Use the latest LTS

```bash
nvm use --lts
```

### List installed versions

```bash
nvm ls
```

### List available versions to install

```bash
nvm ls-remote
```


## Ubuntu installation

It is better to use NVM to install Nodejs. See above.

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
