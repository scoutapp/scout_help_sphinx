# Scout Ruby Config Load Order

The Ruby agent can be configured via a `config/scout_apm.yml` Yaml file and/or environment variables. A config file with your organization key is available for download as part of the install instructions.

Environment variables override settings provided in the `scout_apm.yml` file.

## Config file

The most common way to configure your application is using the `scout_apm.yml` config file. The path of this configuration file is `[PROJECT_ROOT]/config/scout_apm.yml`.

A minimal, working `scout_apm.yml` config file:

```yaml
# config/scout_apm.yml
common: &defaults
  # key: Your Organization key for Scout APM. Found on the settings screen.
  # - Default: none
  key: [YOUR KEY]

  # log_level: Verboseness of logs.
  # - Default: 'info'
  # - Valid Options: debug, info, warn, error
  # log_level: debug

  # name: Application name in APM Web UI
  # - Default: the application names comes from the Rails or Sinatra class name
  # name:

  # monitor: Enable Scout APM or not
  # - Default: none
  # - Valid Options: true, false
  monitor: true

production:
  <<: *defaults

development:
  <<: *defaults
  monitor: false

test:
  <<: *defaults
  monitor: false

staging:
  <<: *defaults
```

### ERB evaluation

ERB is evaluated when loading the config file. For example, you can set the app name based on the hostname:

```yaml
common: &defaults
  name: <%= "ProjectPlanner.io (#{Rails.env})" %>
```

## Environment Variables

You can also configure Scout APM via environment variables. _Environment variables override settings provided in_ `scout_apm.yml`.

Environment variables have the same names as those in the Yaml config file, but are prefixed with `SCOUT_`.

A minimal working configuration of Scout via environment variables:

```ruby
export SCOUT_KEY=YOURKEY
export SCOUT_MONITOR=true
# Start your app here
```