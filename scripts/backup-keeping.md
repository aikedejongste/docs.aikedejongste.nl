---
layout: default
title: Backup Keeping
has_children: false
parent: Scripts
---

## Backup Keeping

```ruby
require 'net/http'
require 'json'
require 'csv'
require 'date'

# Read access token from pat.txt
access_token = File.read('pat.txt').strip
base_url = 'https://api.keeping.nl/v1'
headers = {
  "Authorization" => "Bearer #{access_token}",
  "Accept" => "application/json"
}

# Function to make a GET request to the Keeping API
def api_get_request(url, headers)
  uri = URI(url)
  http = Net::HTTP.new(uri.host, uri.port)
  http.use_ssl = true
  request = Net::HTTP::Get.new(uri, headers)
  response = http.request(request)
  JSON.parse(response.body)
end

def retrieve_organisations(headers)
  url = "https://api.keeping.nl/v1/organisations"
  api_get_request(url, headers)
end

# Function to retrieve a report with pagination
def retrieve_report(organisation_id, from_date, to_date, row_type, headers, base_url)
  all_time_entries = []
  current_page = 1
  last_page = nil

  loop do
    url = "#{base_url}/#{organisation_id}/report/time-entries?from=#{from_date}&to=#{to_date}&row_type=#{row_type}&per_page=100&page=#{current_page}"
    response = api_get_request(url, headers)
    all_time_entries.concat(response["time_entries"])
    last_page ||= response["meta"]["last_page"]
    break if current_page >= last_page
    current_page += 1
  end

  all_time_entries
end

def write_csv(time_entries, organisation_name)
  # Write report data to CSV
  CSV.open("time_entries_#{organisation_name}.csv", "wb") do |csv|
    # Check if there are any time entries
    if time_entries.any?
      # Write headers based on the keys of the first time entry
      csv << time_entries.first.keys
      # Write data rows
      time_entries.each do |entry|
        csv << entry.values
      end
    end
  end
end

response = retrieve_organisations(headers)
organisations = response["organisations"]

organisations.each do |organisation|
  organisation_id = organisation['id']
  organisation_name = organisation['name']
  from_date = (Date.today << 13).strftime("%Y-%m-%d")
  to_date = Date.today.strftime("%Y-%m-%d")
  row_type = 'project' # Replace with your desired row type

  time_entries = retrieve_report(organisation_id, from_date, to_date, row_type, headers, base_url)
  write_csv(time_entries, organisation_name)
end
```
