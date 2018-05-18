## Scout Ruby Environment-Specific Settings

It typically makes sense to treat each environment (production, staging, etc) as a separate application within Scout and ignore the development and test environments. Configure a unique app name for each environment as Scout aggregates data by the app name.

There are 2 approaches:

### 1. Modifying your scout_apm.yml config file

Here's an example `scout_apm.yml` configuration to achieve this:

```yaml
common: &defaults
  name: <%= "YOUR_APP_NAME (#{Rails.env})" %>
  key: YOUR_KEY
  log_level: info
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

### 2. Setting the SCOUT_NAME environment variable

Setting the `SCOUT_NAME` and `SCOUT_MONITOR` environment variables will override settings settings your `scout_apm.yml` config file.

To isolate data for a staging environment: `SCOUT_NAME="YOUR_APP_NAME (Staging)"`.

To disable monitoring: `SCOUT_MONITOR=false`.

See the full list of [configuration options](#ruby-configuration-options).

#### Disabling a Node

To disable Scout APM on any node in your environment, just set `monitor: false` in your `scout_apm.yml` configuration file on that server, and restart your app server. Example:

```yaml
common: &defaults
  name: <%= "YOUR_APP_NAME (#{Rails.env})" %>
  key: YOUR_KEY
  log_level: info
  monitor: false

production:
  <<: *defaults
```

Since the YAML config file allows ERB evaluation, you can even programatically enable/disable nodes based on host name. This example enables Scout APM on web1 through web5:

```yaml
common: &defaults
  name: <%= "YOUR_APP_NAME (#{Rails.env})" %>
  key: YOUR_KEY
  log_level: info
  monitor: <%= Socket.gethostname.match(/web[1..5]/) %>

production:
  <<: *defaults
```

Aft you've disabled a node in your configuration file and restarted your app server, the node show up as inactive in the UI after 10 minutes.