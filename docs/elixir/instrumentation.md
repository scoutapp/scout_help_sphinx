# Scout Elixir Instrumented Libraries

Our [install instructions](#elixir-install) walk through instrumenting the following libraries:

* Phoenix
  * controllers
  * views
  * templates
* Ecto 2.0
* Slime Templates

See [instrumenting common librariess](/#instrumenting-common-libraries) for guides on instrumenting other Elixir libraries.

## Instrumenting Common Elixir Libraries

We've collected best practices for instrumenting common transactions and timing functions below. If you have a suggestion, [please share it](mailto:support@scoutapp.com). See our [custom instrumentation quickstart](#elixir-custom-instrumentation) for more details on adding instrumentation.

* Transactions
  * [Phoenix Channels](#phoenix-channels)
  * [GenServer calls](#genserver-calls)
  * [Task.start](#task-start)
  * [Task.Supervisor.start_child](#task-supervisor-start_child)
  * [Exq](#exq)
  * [Absinthe (GraphQL)](#absinthe)
* Timing
  * [HTTPoison](#httpoison)
  * [MongoDB Ecto](#mongodb-ecto)

### Phoenix Channels

#### Web or background transactions?

* __web__: For channel-related functions that impact the user-facing experience. Time spent in these transactions will appear on your app overboard dashboard and appear in the "Web" area of the UI.
* __background__: For functions that don't have an impact on the user-facing experience (example: click-tracking). These will be available in the "Background Jobs" area of the UI.

#### Naming channel transactions

Provide an identifiable name based on the message the `handle_out/` or `handle_in/` function receives.

An example:

```elixir
defmodule FirestormWeb.Web.PostsChannel do
  use FirestormWeb.Web, :channel
  use ScoutApm.Tracing

  # Will appear under "Web" in the UI, named "PostsChannel.update"
  @transaction(type: "web", name: "PostsChannel.update")
  def handle_out("update", msg, socket) do
    push socket, "update", FetchView.render("index.json", msg)
  end
```

### GenServer calls

It's common to use `GenServer` to handle background work outside the web request flow. Suggestions:

* Treat these as `background` transactions
* Provide a `name` based on the message each `handle_call/` function handles.

An example:

```elixir
defmodule Flight.Checker do
  use GenServer
  use ScoutApm.Tracing

  # Will appear under "Background Jobs" in the UI, named "Flight.handle_check".
  @transaction(type: "background", name: "check")
  def handle_call({:check, flight}, _from, state) do
    # Do work...
  end
```

### Task.start

These execute asynchronously, so treat as a `background` transaction.

```elixir
Task.start(fn ->
  # Will appear under "Background Jobs" in the UI, named "Crawler.crawl".
  ScoutApm.Tracing.transaction(:background,"Crawler.crawl") do
    Crawler.crawl(url)
  end
end)
```

### Task.Supervisor.start_child

Like `Task.start`, these execute asynchronously, so treat as a `background` transaction.

```elixir
Task.Supervisor.start_child(YourApp.TaskSupervisor, fn ->
  # Will appear under "Background Jobs" in the UI, named "Crawler.crawl".
  ScoutApm.Tracing.transaction(:background,"Crawler.crawl") do
    Crawler.crawl(url)
  end
end)
```

### Exq

To instrument [Exq](https://github.com/akira/exq) background jobs, `use ScoutApm.Tracing`, and add a `@transaction` module attribute to each worker's `perform` function:

```elixir
defmodule MyWorker do
  use ScoutApm.Tracing

  # Will appear under "Background Jobs" in the UI, named "MyWorker.perform".
  @transaction(type: "background")
  def perform(arg1, arg2) do
    # do work
  end
end
```

### Absinthe

Requests to the Absinthe plug can be grouped by the GraphQL `operationName` under the "Web" UI by adding <a href="https://gist.github.com/itsderek23/d9435b21c9a44cd9629e93c4e8c2750e" target="_blank">this plug</a> to your pipeline.  

### HTTPoison

Download this <a href="https://gist.github.com/itsderek23/048eaf813af4a1a31a219d75221eb7b7" target="_blank">Demo.HTTPClient module</a> (you can rename to something more fitting) into your app's `/lib` folder, then `alias Demo.HTTPClient` when calling `HTTPoison` functions:

```elixir
defmodule Demo.Web.PageController do
  use Demo.Web, :controller
  # Will route function calls to `HTTPoision` through `Demo.HTTPClient`, which times the execution of the HTTP call.
  alias Demo.HTTPClient

  def index(conn, _params) do
    # "HTTP" will appear on timeseries charts. "HTTP/get" and the url "https://cnn.com" will appear in traces.
    case HTTPClient.get("https://cnn.com") do
      {:ok, %HTTPoison.Response{} = response} ->
        # do something with response
        render(conn, "index.html")
      {:error, %HTTPoison.Error{} = error} ->
        # do something with error
        render(conn, "error.html")
    end
    HTTPClient.post("https://cnn.com", "")
    HTTPClient.get!("http://localhost:4567")
    render(conn, "index.html")
  end
end
```

### MongoDB Ecto

Download <a href="https://gist.github.com/itsderek23/051327a152bc4d95451fd76808b8e83f" target="_blank">this example MongoDB Repo module</a> to use inplace of your existing MongoDB Repo module.

<h2 id="elixir-custom-instrumentation">Custom Instrumentation</h2>

You can extend Scout to record additional types of transactions (background jobs, for example) and time the execution of code that fall outside our custom instrumentation.

For full details on instrumentation functions, see our <a href="https://hexdocs.pm/scout_apm/ScoutApm.Tracing.html" target="_blank">ScoutApm.Tracing Hex docs</a>.