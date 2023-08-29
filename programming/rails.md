---
layout: default
title: Ruby on Rails
has_children: false
parent: Programming
---

# Ruby on Rails

## Dependencies on Ubuntu with MySQL

```bash
apt install curl ufw fail2ban git-core apt-transport-https ca-certificates \
            software-properties-common python3-pip virtualenv python3-setuptools \
            vnstat screen htop tree yarn nodejs redis-server vim gnupg2 imagemagick \
            libmagickwand-dev zlib1g-dev  build-essential  libssl-dev \
            libreadline-dev  libyaml-dev  libxml2-dev  libxslt1-dev  \
            libcurl4-openssl-dev libffi-dev dirmngr gnupg autoconf bison \
            libreadline6-dev libncurses5-dev libmysqlclient-dev mysql-server-5.7 \
            rbenv ruby2.3 ruby2.3-dev bundler python3-certbot-apache
```

## For PG gem

```bash
apt-get install libpq-dev
```

## Ruby with rbenv

DO NOT USE apt package.

```bash
git clone https://github.com/rbenv/rbenv.git ~/.rbenv && echo 'eval "$(~/.rbenv/bin/rbenv init - bash)"' >> ~/.bashrc
```

```bash
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
```

```bash
rbenv install 1.2.3 && rbenv local 1.2.3
```
