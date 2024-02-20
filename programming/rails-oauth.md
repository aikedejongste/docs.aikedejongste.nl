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

## Configure Devise

```ruby
config.omniauth :google_oauth2, Rails.application.credentials.google&.dig("oauth_client_id"), Rails.application.credentials.google&.dig("oauth_client_secret"), {}
```

DO NOT CONFIGURE the omni_auth initializer
ONLY use it for the post or get request setting.

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

## User model

```ruby
...
:omniauthable, omniauth_providers: [:google_oauth2]

  def self.from_omniauth(access_token)
    data = access_token.info
    user = User.where(email: data['email']).first

    return user if user

    User.create(name: data['name'],
                email: data['email'],
                admin: data['email'].split("@").last.downcase == 'domain.nl',
                password: Devise.friendly_token[0, 30])
  end
```


## Redirect_url and Redirect_uri

- it is better to use 127.0.0.1 than localhost as a redirect url
- it is better to use https in development for the redirect url

