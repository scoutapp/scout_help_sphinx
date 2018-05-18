# Scout Elixir Installation

Tailored instructions are provided within our user interface. General instructions for a Phoenix 1.3+ app:

<table class="help install install_ruby">
  <tbody>
    <tr>
      <td>
        <span class="step">1</span>
      </td>
      <td>
Add the `scout_apm` dependency:

Your `mix.exs` file:

```elixir
# mix.exs

 def application do
   [mod: {YourApp, []},
    applications: [..., <span>:scout_apm</span>]]
 end

 def deps do
   [{:phoenix, "~> 1.2.0"},
    ...
    <span>{:scout_apm, "~> 0.0"}</span>]
 end
 ```

Shell:

```
mix deps.get
```
</td>
</tr>

<!-- step 2 -->

<tr>
      <td>
        <span class="step">2</span>
      </td>
      <td>

 Download your customized config file, placing it at <code>config/scout_apm.exs</code>.    

 <p class="smaller">Your customized config file is available within your Scout account. Inside the file, replace <code>"YourApp"</code> with the app name you'd like to appear within Scout.</p>

</td>
</tr>

<!-- step 3 -->

<tr>
      <td>
        <span class="step">3</span>
      </td>
      <td>

Integrate into your Phoenix app.

Instrument Controllers. In lib/your_app_web.ex:

```
# lib/your_app_web.ex
defmodule YourApp.Web do
  def controller do
    quote do
      use Phoenix.Controller
      use ScoutApm.Instrumentation
      ...
```

Instrument Ecto queries. In config/config.exs:

```
# config/config.exs
import_config "scout_apm.exs"

config :your_app, YourApp.Repo,
  loggers: [{Ecto.LogEntry, :log, []},
            {ScoutApm.Instruments.EctoLogger, :log, []}]
```

Instrument Templates. In config/config.exs:


```
# config/config.exs
config :phoenix, :template_engines,
  eex: ScoutApm.Instruments.EExEngine,
  exs: ScoutApm.Instruments.ExsEngine
```
</td>
</tr>

<tr>
      <td>
        <span class="step">4</span>
      </td>
      <td>

Restart your app.

```
mix phoenix.server
```
</td>
</tr>

</tbody>
</table>