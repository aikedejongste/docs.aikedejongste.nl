---
layout: default
title: Rails - Livereload
has_children: false
parent: Programming
---

# Ruby on Rails Livereload

## Add Gems

```bash
bundle add guard
bundle add rack-livereload
```

## Init Guard

```bash
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
