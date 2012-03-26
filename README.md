# SendgridPostback

SendgridPostback is a Rails Engine which integrates with the [SendGrid](http://sendgrid.com)
[Event API](http://docs.sendgrid.com/documentation/api/event-api/).

It includes a MailInterceptor which will attach a UUID header in all mails before they are sent.
When properly configured, SendGrid will then post events for each message to your app. You'll
know when emails are delivered, bounced, delayed, clicked, etc., according to your SendGrid 
account configuration.

Note that for performance reasons, you'll probably want to configure your Event API to batch events.
The bad news is that SendGrid POSTs newline-separate JSON objects, rather than a JSON array. As such,
ActionDispatch is patched (see action_dispatch_ext.rb) to handle the "invalid" incoming JSON.

## Installation

Add this line to your application's Gemfile:

    gem 'sendgrid_postback', git: "git://github.com/aceofspades/sendgrid_postback.git"

You'll need to configure your SendGrid account to enable the Event API.

Configure the library, i.e. in your app's `config/initializers/sendgrid_postback.rb`:

    require 'sendgrid_postback'
    SendgridPostback.configure do |config|
      config.logger = Rails::Logger

      # Path that routes to SendgridPostback::EventsController#create
      config.postback_path = '/sendgrid_postback/events'

      # proc that accepts an exception for reporting
      config.report_exception = proc { |exc| ... } # Optional

      # proc that returns an instance for the given uuid.
      # The class should mix in SendgridPostback::EventReceiver
      config.find_receiver_by_uuid = proc { |uuid| ...} # Required
    end

## Usage

TODO: Write usage instructions here

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request