---
layout: default
title: Ruby on Rails
has_children: false
parent: Programming
---

# Ruby on Rails

## Links

- [LiteStack](https://blog.appsignal.com/2023/09/27/an-introduction-to-litestack-for-ruby-on-rails.html) - hosting with SQLite
- [LiteStack on HN](https://news.ycombinator.com/item?id=37672692)
- [Navigate back](https://dev.to/notapatch/rails-going-backwards-56h5)

## Execute SQL on Rails console

```bash
result = ActiveRecord::Base.connection.execute("SELECT * FROM users")
```

## Good_job Postgres Threads

In database.yaml

```
  pool: <%= $PROGRAM_NAME.include?("good_job") ? ENV.fetch("GOOD_JOB_MAX_THREADS", 5).to_i + 3 : ENV.fetch("RAILS_MAX_THREADS", 5).to_i %>
```

## Dependencies on Ubuntu with MySQL

```bash
apt install curl ufw fail2ban git-core apt-transport-https ca-certificates \
            software-properties-common python3-pip virtualenv python3-setuptools \
            vnstat screen htop tree yarn nodejs redis-server vim gnupg2 imagemagick \
            libmagickwand-dev zlib1g-dev  build-essential  libssl-dev \
            libreadline-dev  libyaml-dev  libxml2-dev  libxslt1-dev  \
            libcurl4-openssl-dev libffi-dev dirmngr gnupg autoconf bison \
            libreadline6-dev libncurses5-dev libmysqlclient-dev mysql-server-5.7 \
            rbenv ruby2.3 ruby2.3-dev bundler python3-certbot-apache \

```

## New list of dependencies

- libmagic-dev for ruby-filemagic
- libvips for ..

```bash
apt install libmagic-dev \
            libvips

pip3 install -U numpy pandas
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

## Edit credentials

```bash
EDITOR="vim" bin/rails credentials:edit --environment staging
```

Check the result with:
`Rails.configuration.database_configuration` on the Rails console.

## Generate Master Key

```bash
bundle exec rails secret | cut -c-32
```

## Secret_key_base

You need to set one for production environments. Just do this and it will be created.

```bash
EDITOR=vim bundle exec rails edit:credentials
```

## constants.rb
