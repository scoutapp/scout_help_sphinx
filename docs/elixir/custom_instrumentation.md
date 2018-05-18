# Scout Elixir Custom Instrumentation

## Transactions & Timing

Scout’s instrumentation is divided into 2 areas:

1. __Transactions__: these wrap around a flow of work, like a web request or a GenServer call. The UI groups data under transactions. Use the `deftransaction/2` macro or wrap blocks of code with the `transaction/4` macro.
2. __Timing__: these measure individual pieces of work, like an HTTP request to an outside service or an Ecto query, and displays timing information within a transaction trace. Use the `deftiming/2` macro or the `timing/4` macro.

## Instrumenting transactions

### deftransaction Macro Example

Replace your function `def` with `deftransaction` to instrument it. You can override the name and type by setting the `@transaction_opts` attribute right before the function.

```elixir
defmodule YourApp.Web.RoomChannel do
  use Phoenix.Channel
  use ScoutApm.Tracing

  # Will appear under "Web" in the UI, named "YourApp.Web.RoomChannel.join".
  @transaction_opts [type: "web"]
  deftransaction join("topic:html", _message, socket) do
    {:ok, socket}
  end

  # Will appear under "Background Jobs" in the UI, named "RoomChannel.ping".
  @transaction_opts [type: "background", name: "RoomChannel.ping"]
  deftransaction handle_in("ping", %{"body" => body}, socket) do
    broadcast! socket, "new_msg", %{body: body}
    {:noreply, socket}
  end
```

### transaction/4 Example

Wrap the block of code you'd like to instrument with `transaction/4`:

```elixir
import ScoutApm.Tracking

def do_async_work do
  Task.start(fn ->
    # Will appear under "Background Jobs" in the UI, named "Do Work".
    transaction(:background, "Do Work") do
      # Do work...
    end
  end)
end
```

See the <a href="https://hexdocs.pm/scout_apm/ScoutApm.Tracing.html" target="_blank">ScoutApm.Tracing Hexdocs</a> for details on instrumenting transactions.

## Timing functions and blocks of code

### deftiming Macro Example

Replace your function `def` with `deftiming` to instrument it. You can override the name and category by setting the `@timing_opts` attribute right before the function.


```elixir
defmodule Searcher do
  use ScoutApm.Tracing

  # Time associated with this function will appear under "Hound" in timeseries charts.
  # The function will appear as `Hound/open_search` in transaction traces.
  @timing_opts [category: "Hound"]
  deftiming open_search(url) do
    navigate_to(url)
  end

  # Time associated with this function will appear under "Hound" in timeseries charts.
  # The function will appear as `Hound/homepage` in transaction traces.
  @timing_opts [category: "Hound", name: "homepage"]
  deftiming open_homepage(url) do
    navigate_to(url)
  end
```

### timing/4 Example

Wrap the block of code you'd like to instrument with `timing/4`:

```elixir
defmodule PhoenixApp.PageController do
  use PhoenixApp.Web, :controller
  import ScoutApm.Tracing

  def index(conn, _params) do
    timing("Timer", "sleep") do
      :timer.sleep(3000)
    end
    render conn, "index.html"
  end
```

See the <a href="https://hexdocs.pm/scout_apm/ScoutApm.Tracing.html" target="_blank">ScoutApm.Tracing Hexdocs</a> for details on timing functions and blocks of code.

### Limits on category arity

We limit the number of unique categories that can be instrumented. Tracking too many unique category can impact the performance of our UI. Do not dynamically generate categories in your instrumentation (ie `timing("user_#{user.id}", "generate", do: do_work())` as this can quickly exceed our rate limits.

### Adding a description

Call `ScoutApm.Tracing.update_desc/1` to add relevant information to the instrumented item. This description is then viewable in traces. An example:

```elixir
timing("HTTP", "GitHub_Avatar") do
  url = "https://github.com/#{user.id}.png"
  update_desc("GET #{url}")
  HTTPoison.get(url)
end
```

### Tracking already executed time

Libraries like Ecto log details on executed queries. This includes timing information. To add a trace item for this, use `ScoutApm.Tracing.track`. An example:

```elixir
defmodule YourApp.Mongo.Repo do
  use Ecto.Repo

  # Scout instrumentation of Mongo queries. These appear in traces as "Ecto/Read", "Ecto/Write", etc.
  def log(entry) do
    ScoutApm.Tracing.track(
      "Ecto",
      query_name(entry),
      entry.query_time,
      :microseconds
    )
    super entry
  end

end
```

In the example above, the metric will appear in traces as `Ecto/#{query_time(entry)}`. On timeseries charts, the time will be allocated to `Ecto`.

<a href="https://hexdocs.pm/scout_apm/ScoutApm.Tracing.html#track/5" target="_blank">See the scout_apm hex docs</a> for more information on `track/`.

<h3 id="elixir-testing-instrumentation">Testing instrumentation</h3>

Improper instrumentation can break your application. It's important to test before deploying to production. The easiest way to validate your instrumentation is by running [DevTrace](#elixir-devtrace) and ensuring the new metric appears as desired.

After restarting your app with DevTrace enabled, refresh the browser page and view the trace output. The new metric should appear in the trace.
