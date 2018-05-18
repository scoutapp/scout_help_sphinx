# Monitoring Ruby Frameworks with Scout

## Ruby on Rails

Scout auto-instruments both web and background jobs for Ruby on Rails 2.2+. No additional configuration is required.

### ActionController::Metal

Prior to agent version 2.1.26, an extra step was required to instrument `ActionController::Metal`
and <span style="white-space: nowrap;">`ActionController::Api`</span> controllers. After 2.1.26, this is automatic.

The previous instructions which had an explicit `include` are no longer
needed, but if that code is still in your controller, it will not harm
anything. It will be ignored by the agent and have no effect.


## Rack

Rack instrumentation is more explicit than Rails instrumentation, since Rack applications can take
nearly any form. After [installing our agent](installation.html), instrumenting Rack is a three step process:

1. Configuring the agent
2. Starting the agent
3. Wrapping endpoints in tracing

<h3 id="rack-config">Configuration</h3>

Rack apps are configured using the same approach as a Rails app: either via a `config/scout_apm.yml` config file or environment variables.

* __configuration file__: create a `config/scout_apm.yml` file under your application root directory. The file structure is outlined [here](config_reference.html).
* __environment variables__: see our docs on configuring the agent via [environment variables](#ruby-env-vars).

### Starting the Agent

Add the `ScoutApm::Rack.install!` startup call as close to the spot you
`run` your Rack application as possible.  <span style="white-space: nowrap;">`install!`</span>
should be called after you require other gems (ActiveRecord, Mongo, etc)
to install instrumentation for those libraries.

```ruby
# config.ru

require 'scout_apm'
ScoutApm::Rack.install!

run MyApp
```

### Adding endpoints

Wrap each endpoint in a call to `ScoutApm::Rack#transaction(name, env)`.

* `name` - an unchanging string argument for what the endpoint is. Example: `"API User Listing"`
* `env` - the rack environment hash

This may be fairly application specific in details.

Example:

```ruby
app = Proc.new do |env|
  ScoutApm::Rack.transaction("API User Listing", env) do
    User.all.to_json
    ['200', {'Content-Type' => 'application/json'}, [users]]
  end
end
```

If you run into any issues, or want advice on naming or wrapping endpoints, contact us at
support@scoutapp.com for additional help.

## Sinatra

Instrumenting a Sinatra application is similar to instrumenting a generic [Rack application](#rack).

<h3 id="sinatra-configuration">Configuration</h3>

The agent configuration (API key, app name, etc) follows the same process as the [Rack application config](#rack-config).

<h3 id="sinatra-starting-agent">Starting the agent</h3>

Add the `ScoutApm::Rack.install!` startup call as close to the spot you
`run` your Sinatra application as possible.  <span style="white-space: nowrap;">`install!`</span>
should be called _after_ you require other gems (ActiveRecord, Mongo, etc).

```ruby
require './main'

require 'scout_apm'
ScoutApm::Rack.install!

run Sinatra::Application
```

<h2 id="sinatra-transactions">Adding endpoints</h2>

Wrap each endpoint in a call to `ScoutApm::Rack#transaction(name, env)`. For example:

```ruby
get '/' do
  ScoutApm::Rack.transaction("get /", request.env) do
    ActiveRecord::Base.connection.execute("SELECT * FROM pg_catalog.pg_tables")
    "Hello!"
  end
end
```

See our [Rack docs for adding an endpoint](#adding-endpoints) for more details.

## Sneakers

Scout doesn't instrument [Sneakers](https://github.com/jondot/sneakers) (a background processing framework for Ruby and RabbitMQ) automatically. To add Sneakers instrumentation:

* [Download the contents of this gist](https://gist.github.com/itsderek23/685c7485a3bd020b6cdd9b1d61cb847f). Place the file inside your application's `/lib` folder or similar.
* In `config/boot.rb`, add: `require File.expand_path('lib/scout_sneakers.rb', __FILE__)`
* In your `Worker` class, immediately following the `work` method, add<br/>`include ScoutApm::BackgroundJobIntegrations::Sneakers::Instruments`.

This treats calls to the `work` method as distinct transactions, named with the worker class.

Example usage:

```ruby
class BaseWorker
  include Sneakers::Worker

  def work(attributes)
    # Do work
  end
  # This MUST be included AFTER the work method is defined.
  include ScoutApm::BackgroundJobIntegrations::Sneakers::Instruments
end
```
