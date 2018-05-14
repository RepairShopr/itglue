# ITGlue

A wrapper for the [ITGlue API](https://api.itglue.com/developer/).

## Installation

```ruby
gem 'itglue'
```

## Requirements

* Ruby 2.0.0 or higher

## Setup

### Authentication

For now this gem only supports API Key authentication.

### Configuration

```ruby
require 'itglue'

ITGlue.configure do |config|
  config.itglue_api_key = 'ITG.b94e901420fe7cb163a364b451f172d9.P3nMIV9EDp1Xb7h4-sO7WDU0S2R5DgC-fu2DsTxBkW7eZHRp0y4ODRQ51se1c24m' config.itglue_api_base_uri = 'https://api.itglue.com'
  config.logger = ::Logger.new(STDOUT)
end
```

## Usage

### Basics

Get all organizations
```ruby
organizations = ITGlue::Organization.get
#=> [#<ITGlue::Organization id: 123 name: "Happy Frog", description: nil, ...>, <ITGlue::Organization id: 124 name: "Vancity Superstars", description: nil, ...>, ...]
```
Get organizations with a filter
```ruby
organizations = ITGlue::Organization.filter(name: 'Happy Frog')
#=> [#<ITGlue::Organization id: 123 name: "Happy Frog", description: nil, ...>]
```
Get organization by id
```ruby
organization = ITGlue::Organization.find(123)
#=> #<ITGlue::Organization id: 123 name: "Ben's Gem Test", description: nil, ...>]
```
Get configurations for a specific organization
```ruby
configuration = ITGlue::Organization.get_nested(organization)
```

### Client

You can also directly instantiate a client and handle the data and response directly.
```ruby
client = ITGlue::Client.new
#=> #<ITGlue::Client:0x007fd7eb032d00 ...>
query = { filter: { name: 'HP' } }
client.get(:configurations, { parent: organization }, { query: query })
# => [
#   {
#     id: 456,
#     type: "configurations",
#     attributes: {
#       organization_id: 123,
#       organization_name: "Happy Frog",
#       ...
#     }
#   },
#   ...
# ]
```
A get request such as the one above will handle generating the route and pagination. If you want to handle these yourself, you can use 'execute'
```ruby
client.execute(:get, '/organizations/31131/relationships/configurations', nil, {query: {filter: {name: 'HP'}}})
# => {
#   "data"=> [
#     {
#       "id"=>"23943",
#       "type"=>"configurations",
#       "attributes"=> {
#         "organization-id"=>31131,
#         "organization-name"=>"Ben's Gem Test",
#         ...
#       }
#     },
#     ...
#   ],
#   "links"=>{},
#   "meta"=>{"current-page"=>1, "next-page"=>nil, "prev-page"=>nil, "total-pages"=>1, "total-count"=>1, "filters"=>{}}
# }
```

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/b-loyola/itglue.

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
