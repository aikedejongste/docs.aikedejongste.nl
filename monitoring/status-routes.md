---
layout: default
title: Status routes
has_children: false
parent: Monitoring
---

# Status routes

This is a simple way to add very basic monitoring to a web app. As status route is a public url that
returns a http status code and a text with the status of the app. The developer can add basic
checks. Like checking if the database is online, workers are active, env vars are set, there is
enough diskspace, etc. Again this is just the bare necessities and full server monitoring is better
of course. Add these to simple website monitoring tools. Your site or app may be online but not
sending email because a worker is not running and this a simple way to detect that.

## Rails sidekiq check

From: [RunRails.com](https://www.runrails.com/monitoring/monitoring-sidekiq-with-alerts/)
Also interesting: [Sidekiq checks](https://pawelurbanek.com/rails-sidekiq-monitoring)

```ruby
class Public::StatusController < Public::PublicController
  # only if you use Pundit
  before_action :skip_authorization

  def database
    Account.count
    render plain: "Database is okay", status: :ok
  rescue StandardError
    render(plain: "Can't query database", status: :service_unavailable) && return
  end

  def sidekiq
    stats = Sidekiq::Stats.new
    latency = Sidekiq::Queue.new.latency

    render(plain: 'No processes',                              status: :service_unavailable) && return if stats.processes_size == 0
    render(plain: "Too many enqueued (#{stats.enqueued})",     status: :service_unavailable) && return if stats.enqueued > 250
    render(plain: "Latency more than 10 minutes (#{latency})", status: :service_unavailable) && return if latency > 600
    render plain: 'Sidekiq is alive and kicking',              status: :ok
  end
end
```
