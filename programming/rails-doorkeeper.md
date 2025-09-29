---
layout: default
title: Rails - Doorkeeper
has_children: false
parent: Programming
---

# Rails and Doorkeeper

## Summary

By adding the Doorkeeper Gem to a Rails app it can be made into the central identity provider for other applications in the organisation.
Users can login on other applications like Grafana with the same credentials as on the Rails app. The User model in Rails is
leading here.

## Links

- [DoorKeeper](https://github.com/doorkeeper-gem/doorkeeper)
- [DoorKeeper OpenID](https://github.com/doorkeeper-gem/doorkeeper-openid_connect)
- [Grafana Generic Oauth](https://grafana.com/docs/grafana/latest/setup-grafana/configure-security/configure-authentication/generic-oauth/)

## Rails

In config/routes.rb

```yaml
  use_doorkeeper_openid_connect
  use_doorkeeper
```

### The Doorkeeper initializer

Set `default_scopes :openid` in config/initializers/doorkeeper.rb.

and

```yaml
  signing_key Rails.application.credentials.oidc_key

  resource_owner_from_access_token do |access_token|
    user = User.find_by(id: access_token.resource_owner_id)
    User.find_by(id: access_token.resource_owner_id)
  end

  claims do
    normal_claim :email, scope: :openid do |resource_owner|
      resource_owner.email
    end

    normal_claim :full_name, scope: :openid do |resource_owner|
      resource_owner.name
    end

    # Role for Grafana
    normal_claim :role, scope: :openid do |resource_owner|
      if resource_owner.admin?
        'Admin'
      else
        'Viewer'
      end
      end
    end
  end

```

### The orgs endpoint for Grafana

In `app/controllers/user_info_controller.rb`:

```yaml
class UserInfoController < ApplicationController
  def orgs
    render json: [
      {
        "id": 1,
        "name": "Example Organization 1",
        "role": "admin"
      }
    ]

  end
end
```

## Rails - Grafana config

Let Rails redirect to `/login/generic_oauth`

## Grafana config

Grafana will still have local users.

```yaml
[users]
allow_sign_up = false
allow_org_create = false
auto_assign_org = true
auto_assign_org_id = 1
auto_assign_org_role = Viewer
verify_email_enabled = false

[auth.generic_oauth]
name = YourCompany Account
icon = signin
enabled = true
allow_sign_up = true
client_id = abc23456789
client_secret = def1235670
scopes = openid
empty_scopes = false
email_attribute_name = email
auth_url = https://you.ngrok-free.app/oauth/authorize
token_url = https://you.ngrok-free.app/oauth/token
api_url = https://you.ngrok-free.app/oauth/userinfo
teams_url =
allowed_domains =
team_ids =
allowed_organizations =
allow_assign_grafana_admin = false
```

## Other

- Grafana has organisation id's.
- You can use ${__user.email} in Grafana dashboard queries to get the users email.
