---
layout: default
title: Github Issues to Todoist
parent: Various scripts
has_children: false
---

# Github issues to Todist

```ruby
require 'net/http'
require 'json'
require 'uri'

def get_assigned_issues(username, token)
  # Construct the API URL
  uri = URI.parse("https://api.github.com/search/issues?q=assignee:#{username}+is:issue")

  # Create the HTTP object
  http = Net::HTTP.new(uri.host, uri.port)
  http.use_ssl = true

  # Create the request object
  request = Net::HTTP::Get.new(uri.request_uri)
  request['Authorization'] = "token #{token}"
  request['User-Agent'] = "MyIssueFetcher"

  # Execute the request
  response = http.request(request)

  # Parse the response
  begin
    data = JSON.parse(response.body)
    return data['items']
  rescue JSON::ParserError
    puts "Failed to parse the response."
    return []
  end
end

def main
  # Read from environment variables or prompt the user if not found
  username = ENV['GITHUB_USERNAME'] || (puts "Enter your GitHub username:" && gets.chomp)
  token = ENV['GITHUB_TOKEN'] || (puts "Enter your GitHub access token:" && gets.chomp)

  issues = get_assigned_issues(username, token)

  if issues && !issues.empty?
    puts "Issues assigned to #{username}:"
    issues.each do |issue|
      puts "#{issue['html_url']} - #{issue['title']}"
    end
  else
    puts "No issues found for #{username}."
  end
end

main
```
