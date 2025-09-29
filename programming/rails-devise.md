---
layout: default
title: Rails - Devise
has_children: false
parent: Programming
---

# Rails - install Devise

## Add devise gem

```bash
bundle add devise
```

## Install Devise

```bash
rails generate devise:install
```

## Generate views

```bash
rails generate devise:views
```

## Generate User model

```bash
rails generate devise User
```

## Migrate db

```bash
rails db:migrate
```

## Add before action

In application controller:

```ruby
    before_action :authenticate_admin

    def authenticate_admin
      unless user_signed_in? && current_user.admin?
        redirect_to root_path, alert: "You are not authorized to access this page"
      end
    end
```

## Add flash messages

- Create `_flash.html.erb` partial.
