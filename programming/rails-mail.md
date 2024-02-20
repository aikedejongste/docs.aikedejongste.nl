---
layout: default
title: Mail - smtp2go
has_children: true
parent: Programming
---

# Rails and email

## smtp2go

1. Sign up (activation link is broken, just login)
2. Add verified sender domain
3. Add dns records
4. Add a user

## Rails

### 0. Theory

There is .deliver_now for mailers.


### 1. put the credentials in the correct credentials

```yaml
smtp_username: app_prod
smtp_password: balbalbala
smtp_address: mail.smtp2go.com
smtp_domain: yourdomain.com
smtp_port: 2525
smtp_from: notifications@yourdomain.com
```

### 2. put config in the correct environments

In `config/environments/development.rb` and `config/environments/production.rb`

```yaml
config.action_mailer.raise_delivery_errors = true
config.action_mailer.default_url_options = { host: Rails.application.credentials.dig(:smtp_domain) }
config.action_mailer.delivery_method = :smtp
config.action_mailer.asset_host = Rails.application.credentials.dig(:smtp_domain)
config.action_mailer.smtp_settings = {
  address:         Rails.application.credentials.dig(:smtp_address),
  port:            Rails.application.credentials.dig(:smtp_port),
  domain:          Rails.application.credentials.dig(:smtp_domain),
  user_name:       Rails.application.credentials.dig(:smtp_username),
  password:        Rails.application.credentials.dig(:smtp_password),
  authentication:  :plain,
  enable_starttls_auto: true,
  enable_starttls: true,
  open_timeout:    5,
  read_timeout:    5
}

config.action_mailer.perform_caching = false
```

Yes, you should use TLS/SSL by default.

### 3. Generate a mailer

```bash
rails generate mailer user_mailer
```

### 4. Open the preview

Visit [http://localhost:3000/rails/mailers](http://localhost:3000/rails/mailers).

If you don't see the preview it might be in the `rspec/mailers` directory instead of in `test/mailers`.
