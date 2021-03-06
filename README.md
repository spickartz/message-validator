# Envelope::MessageValidator
This gem allows for a validation of YAML messages based on [kwalify](http://www.kuwata-lab.com/kwalify/)

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'message-validator'
```

And then execute:

    $ bundle exec bin/message-validator

Or install it yourself as:

    $ gem install message-validator

## Usage

```
Commands:
  message-validator agent           # run as agent to provide validations via MQTT
  message-validator help [COMMAND]  # Describe available commands or one specific command
  message-validator list            # list available schemata
  message-validator validate YAML   # validates if YAML is valid

Options:
  -p, [--schemata-path=SCHEMATA-PATH]  # path to schemata files
                                       # Default: /Users/Simon/work/envelope-validator/schemata
```

### Agent mode

#### Requests
Validation requests have to be posted to the topic:
        
        envelope/validator/validate/<schema>/<id>

whereas `<schema>` is a schema in accordance to the list obtained via `message-validator list`. Passing `.*`, `anyschema`, or leaving the field empty requests a validation against all available schemata. `<id>` is an arbitrary string that can be used for an identification of the result message. The payload of the message should contain a string in [YAML](http://www.yaml.org) format. 

#### Results
The agent publishes results on the topics:

        envelope/validator/result/<id>
        
`<id>` corresponds to the ID defined within the request topic. If the `<id>` field is empty, it is omitted in the result topic as well. Result messages contain the following information:
```
result: <success/failure>
validations:
- schema: <schema name>
  result: <true/false>
- schema: <schema name>
  result: <true/false>
  errors:
  - <error string>
  - <error string>
  - ...
- ...
```
    
    
## Development

After checking out the repo, run `bin/setup` to install dependencies. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/envelope-project/message-validator.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

