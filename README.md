# Fluent::Plugin::Splunk

Splunk output plugin for Fluent event collector.

This plugin makes use of the following APIs:

Splunk REST API:

  http://docs.splunk.com/Documentation/Splunk/latest/RESTAPI/RESTinput

Splunk Storm API:

  http://docs.splunk.com/Documentation/Storm/latest/User/UseStormsRESTAPI

## Installation

Add this line to your application's Gemfile:

    gem 'fluent-plugin-splunk'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install fluent-plugin-splunk

## Usage

Put the following lines to your fluent.conf

    <match **>
      type splunk

      #
      # Splnk Server
      #

      # protocol: API protocol version
      # values: 'rest', 'storm'
      protocol rest

      # server: Splunk server host and port
      server localhost:8089

      # verify: SSL server verification
      #verify false

      # auth: username and password
      auth admin:pass

      #
      # Splnk Storm
      #

      # protocol: API protocol version.
      # values: 'rest', 'storm'
      #protocol storm

      # access_token: for Splunk Storm
      #access_token YOUR-ACCESS-TOKEN

      # access_token: for Splunk Storm
      #project_id YOUR-PROJECT-ID

      #
      # Event Parameters
      #

      # host: 'host' parameter passed to Splunk
      host YOUR-HOSTNAME

      # host: 'source' parameter passed to Splunk
      #
      # "{TAG}" will be replaced by tags at runtime
      source {TAG}

      # sourcetype: 'sourcetype' parameter passed to Splunk
      sourcetype fluent

      #
      # Formatting Parameters
      #

      # time_format: the time format of each event
      # value: 'none', 'unixtime', or time format string
      time_format unixtime

      # format: the text format of each event
      # value: 'json', 'kvp', or 'text'
      #
      # input = {"x":1, "y":"xyz", "message":"Hello, world!"}
      # 
      # 'json' is JSON encoding:
      #   {"x":1,"y":"xyz","message":"Hello, world!"}
      # 
      # 'kvp' is "key=value" pairs, which is automatically detected as fields by Splunk:
      #   x="1" y="xyz" message="Hello, world!"
      # 
      # 'text' outputs the value of "message" as is, with "key=value" pairs for others:
      #   [x="1" y="xyz"] Hello, world!
      format json

      #
      # Buffering Parameters
      #

      # Standard parameters for buffering.  See documentation for details:
      #   http://docs.fluentd.org/articles/buffer-plugin-overview
      buffer_type memory
      buffer_queue_limit 16

      # buffer_chunk_limit: The maxium size of POST data in a single API call.
      # 
      # This value should be reasonablly small since the current implementation
      # of out_splunk converts a chunk to POST data on memory before API calls.
      # The default value should be good enough.
      buffer_chunk_limit 8m

      # flush_interval: The interval of API requests.
      # 
      # Make sure that this value is large enough to make successive API calls.
      # Note that a different source produces a different API POST, each of which
      # costs two or more seconds.  If you include "{TAG}" in the source parameter and
      # this 'match' section recieves many tags, a single flush may take long time.
      # (Run fluentd with -v to see verbose logs.)
      flush_interval 60s
    </match>

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
