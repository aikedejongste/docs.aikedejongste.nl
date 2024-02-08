---
layout: default
title: Rails Livereload
has_children: false
parent: Programming
---

# Ruby on Rails Livereload


guard init livereload

Guardfile:

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


Procfile.dev
guard: bundle exec guard

gem "rack-livereload", group: :development


in config/environments/development.rb
  # Add Rack::LiveReload to the bottom of the middleware stack with the default options:
  config.middleware.insert_after ActionDispatch::Static, Rack::LiveReload

