# Scout Elixir Config File

The Elixir agent can be configured via the `config/scout_apm.exs` file. A config file with your organization key is available for download as part of the install instructions. A application name and key is required:

```
config :scout_apm,
     name: "Your App", # The app name that will appear within the Scout UI
     key: "YOUR SCOUT KEY"
```

Alternately, you can also use environment variables of your choosing by formatting your configuration as a tuple
with :system as the first value and the environment variable expected as the second.

```
config :scout_apm,
  name: { :system, "SCOUT_APP_NAME" },
  key: { :system, "SCOUT_APP_KEY" }
```