# Scout Ruby Docker and PaaS Configuration

## Docker

Scout runs within Docker containers without any special configuration.

It's common to configure Docker containers with environment variables. Scout can use [environment variables](#environment-variables) instead of the `scout_apm.yml` config file.

## Heroku

Scout runs on Heroku without any special configuration. When Scout detects that an app is being served via Heroku:

* Logging is set to `STDOUT` vs. logging to a file. Log messages are prefixed with `[Scout]` for easy filtering.
* The dyno name (ex: `web.1`) is reported vs. the dyno hostname. Dyno hostnames are dynamically generated and don't have any meaningful information.

<h2 id="heroku-configuration">Configuration</h2>

Scout can be configured via environment variables. This means you can use `heroku config:set` to configure the agent. For example, you can set the application name that appears in the Scout UI with:

```bash
heroku config:set SCOUT_NAME='My Heroku App'
```

See the configuration section for more information on the available config settings and environment variable functionality.

## Using the Scout Heroku Add-on

Scout is also available as a [Heroku Add-on](https://elements.heroku.com/addons/scout). The add-on automates setting the proper Heroku config variables during the provisioning process.

## Cloud Foundry

Scout runs on Cloud Foundry without any special configuration.

We suggest a few configuration changes in the `scout_apm.yml` file to best take advantage of Cloud Foundry:

1. Set `log_file_path: STDOUT` to send your the Scout APM log contents to the Loggregator.
2. Use the application name configured via Cloud Foundry to identify the app.
3. Override the hostname reported to Scout. Cloud Foundry hostnames are dynamically generated and don't have any meaningful information. We suggest using a combination of the application name and the instance index.

A sample config for Cloud Foundry that implements the above suggestions:

```yaml
common: &defaults
  key: YOUR_KEY
  monitor: true
  # use the configured application name to identify the app.
  name: <%= ENV['VCAP_APPLICATION'] ? JSON.parse(ENV['VCAP_APPLICATION'])['application_name'] : "YOUR APP NAME (#{Rails.env})" %>
  # make logs available to the Loggregator
  log_file_path: STDOUT
  # reports w/a more identifiable instance name using the application name and instance index. ex: rails-sample.0
  hostname: <%= ENV['VCAP_APPLICATION'] ? "#{JSON.parse(ENV['VCAP_APPLICATION'])['application_name']}.#{ENV['CF_INSTANCE_INDEX']}"  : Socket.gethostname %>

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