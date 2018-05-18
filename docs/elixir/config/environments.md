# Scout Elixir Environment-Specific Settings

It typically makes sense to treat each environment (production, staging, etc) as a separate application within Scout. To do so, configure a unique app name for each environment. Scout aggregates data by the app name.

An example:

```elixir
# config/staging.exs

config :scout_apm,
  name: "YOUR APP - Staging"
```