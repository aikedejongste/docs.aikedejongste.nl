---
layout: default
title: Rails - OmniAuth/OAuth
has_children: false
parent: Programming
---

# Rails - OmniAuth/OAuth

## Add gems

```bash
bundle add omniauth
bundle add omniauth-oauth2
bundle add omniauth_openid_connect
```

optional: `bundle add omniauth-rails_csrf_protection`

## Install Devise

```bash
rails generate devise:install
```

## Generate views

```bash
rails generate devise:views
```

## Add database colums to user model

```bash
rails g migration AddColumnsToUsers provider uid
```

## Migrate db

```bash
rails db:migrate
```

## Controller method

In `app/controllers/users/omniauth_callbacks_controller.rb`

```ruby
  def zitadel
    # You need to implement the method below in your model (e.g. app/models/user.rb)
    @user = User.from_omniauth(request.env["omniauth.auth"])

    if @user.persisted?
      sign_in_and_redirect @user, event: :authentication
      set_flash_message(:notice, :success, kind: "ZITADEL") if is_navigational_format?
    else
      session["devise.zitadel_data"] = request.env["omniauth.auth"]
      redirect_to new_user_registration_url
    end
  end

  def failure
    redirect_to root_path
  end
```
