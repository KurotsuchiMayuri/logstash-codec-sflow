# Logstash Codec SFlow Plugin
## Description
Logstash codec plugin to decode sflow codec.

This codec manage flow sample and counter flow.

For the flow sample it is able to decode Ethernet, 802.1Q VLAN, IPv4, UDP and TCP header

For the counter flow it is able to decode some records of type:

- Generic Interface
- Ethernet Interface
- VLAN
- Processor Information
- HTTP

## TO DO
Currently this plugin does not manage all sflow counter and is not able to decode
all kind of protocols.
If needed you can aks for some to be added.
Please provide a pcap file containing the sflow events of the counter/protocol
to add in order to be able to implement it.

## Human Readable Protocol
In order to translate protocols value to a human readable protocol, you can use the
logstash-filter-translate plugin
```
filter {
      translate {
        field => protocol
        dictionary => [ "1", "ETHERNET",
                        "11", "IP"
                      ]
        fallback => "UNKNOWN"
        destination => protocol
        override => true
      }
      translate {
        field => eth_type
        dictionary => [ "2048", "IP",
                        "33024", "802.1Q VLAN"
                      ]
        fallback => "UNKNOWN"
        destination => eth_type
        override => true
      }
      translate {
        field => vlan_type
        dictionary => [ "2048", "IP"
                      ]
        fallback => "UNKNOWN"
        destination => vlan_type
        override => true
      }
      translate {
        field => ip_protocol
        dictionary => [ "6", "TCP",
                        "17", "UDP"
                      ]
        fallback => "UNKNOWN"
        destination => ip_protocol
        override => true
      }
}
```

[![Build
Status](http://build-eu-00.elastic.co/view/LS%20Plugins/view/LS%20Codecs/job/logstash-plugin-codec-example-unit/badge/icon)](http://build-eu-00.elastic.co/view/LS%20Plugins/view/LS%20Codecs/job/logstash-plugin-codec-example-unit/)

This is a plugin for [Logstash](https://github.com/elastic/logstash).

It is fully free and fully open source. The license is Apache 2.0, meaning you are pretty much free to use it however you want in whatever way.

## Documentation

Logstash provides infrastructure to automatically generate documentation for this plugin. We use the asciidoc format to write documentation so any comments in the source code will be first converted into asciidoc and then into html. All plugin documentation are placed under one [central location](http://www.elastic.co/guide/en/logstash/current/).

- For formatting code or config example, you can use the asciidoc `[source,ruby]` directive
- For more asciidoc formatting tips, see the excellent reference here https://github.com/elastic/docs#asciidoc-guide

## Need Help?

Need help? Try #logstash on freenode IRC or the https://discuss.elastic.co/c/logstash discussion forum.

## Developing

### 1. Plugin Developement and Testing

#### Code
- To get started, you'll need JRuby with the Bundler gem installed.

- Create a new plugin or clone and existing from the GitHub [logstash-plugins](https://github.com/logstash-plugins) organization.

- Install dependencies
```sh
bundle install
```

#### Test

```sh
bundle exec rspec
```

The Logstash code required to run the tests/specs is specified in the `Gemfile` by the line similar to:
```ruby
gem "logstash", :github => "elasticsearch/logstash", :branch => "1.5"
```
To test against another version or a local Logstash, edit the `Gemfile` to specify an alternative location, for example:
```ruby
gem "logstash", :github => "elasticsearch/logstash", :ref => "master"
```
```ruby
gem "logstash", :path => "/your/local/logstash"
```

Then update your dependencies and run your tests:

```sh
bundle install
bundle exec rspec
```

### 2. Running your unpublished Plugin in Logstash

#### 2.1 Run in a local Logstash clone

- Edit Logstash `tools/Gemfile` and add the local plugin path, for example:
```ruby
gem "logstash-codec-sflow", :path => "/your/local/logstash-codec-sflow"
```
- Update Logstash dependencies
```sh
rake vendor:gems
```
- Run Logstash with your plugin
```sh
bin/logstash -e 'input { udp { port => 6343 codec => sflow }}'
```
At this point any modifications to the plugin code will be applied to this local Logstash setup. After modifying the plugin, simply rerun Logstash.

#### 2.2 Run in an installed Logstash

- Build your plugin gem
```sh
gem build logstash-codec-sflow.gemspec
```
- Install the plugin from the Logstash home
```sh
bin/plugin install /your/local/plugin/logstash-codec-sflow.gem
```
- Start Logstash and proceed to test the plugin

## Contributing

All contributions are welcome: ideas, patches, documentation, bug reports, complaints, and even something you drew up on a napkin.

Programming is not a required skill. Whatever you've seen about open source and maintainers or community members  saying "send patches or die" - you will not see that here.

It is more important to me that you are able to contribute.

For more information about contributing, see the [CONTRIBUTING](https://github.com/elastic/logstash/blob/master/CONTRIBUTING.md) file.