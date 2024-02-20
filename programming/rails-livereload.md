---
layout: default
title: Rails - Guard
has_children: false
parent: Programming
---

# Ruby on Rails - Guard Livereload

## Add Gems

```bash
bundle add guard --group development
bundle add rack-livereload --group development
bundle add guard-shell --group development
```

## Init Guard

```bash
bundle exec guard init
bundle exec guard init livereload
```

## Check or create Guardfile

```yaml
group :livereload do
  guard 'livereload' do
    watch(things)
  end
end

guard 'livereload' do
  watch(%r{app/views/.+\.(erb|haml|slim)$})
  watch(%r{app/helpers/.+\.rb})
  watch(%r{public/.+\.(css|js|html)})
  watch(%r{config/locales/.+\.yml})
  # Rails Assets Pipeline
  watch(%r{(app|vendor)(/assets/\w+/(.+\.(css|js|html|png|jpg))).*}) { |m| "/assets/#{m[3]}" }
end

guard :shell do
  watch('Gemfile.lock') { `touch tmp/restart.txt` }
  watch(%r{^(config|lib)/.*}) { `touch tmp/restart.txt` }
  watch('app/controllers/application_controller.rb') { `touch tmp/restart.txt` }
  watch(%r{^Procfile.dev}) { `touch tmp/restart.txt` }
  watch(%r{^Guardfile}) { `touch tmp/restart.txt` }
  watch(%r{config/initializers/.+\.rb}) { `touch tmp/restart.txt` }
end

```

## Enable Guard in Procfile.dev

```bash
guard: bundle exec guard
```

## Update development env

In `config/environments/development.rb`

```ruby
  # Add Rack::LiveReload to the bottom of the middleware stack with the default options:
  config.middleware.insert_after ActionDispatch::Static, Rack::LiveReload
```
