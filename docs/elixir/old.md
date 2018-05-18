

<!-- old reference -->
<a name="enabling-devtrace30"></a>
<h2 id="elixir-devtrace">Enabling DevTrace</h2>

<aside class="notice" style="font-size: 12px">
Follow the <a href="#elixir-install">install instructions</a> first. There's no need to download the config file as signup isn't required.
</aside>

To enable [DevTrace](#devtrace), our in-browser profiler:

<table class="help install">
  <tbody>
    <tr>
      <td><span class="step">1</span></td>
      <td>
        <p style="line-height: 170%">
          Add <code>dev_trace: true</code> to the <code>scout_apm</code> section of your <code>config/dev.exs</code>  file:
        </p>
<pre>
# config/dev.exs
config :scout_apm,
  dev_trace: true
</pre>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">2</span>
      </td>
      <td style="vertical-align: middle">
        <p style="line-height: initial">Restart your app.</p>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">3</span>
      </td>
      <td style="vertical-align: middle">
        <p style="line-height: initial">Refresh your browser window and look for the speed badge.</p>
      </td>
    </tr>
  </tbody>
</table>

<h2 id="elixir-server-timing">Server Timing</h2>

View performance metrics (time spent in Controller, Ecto, etc) for each of your app's requests in Chrome’s Developer tools with the [plug_server_timing](https://github.com/scoutapp/elixir_plug_server_timing) package. Production-safe.

![server timing](https://s3-us-west-1.amazonaws.com/scout-blog/elixir_server_timing.png
)

For install instructions and configuration options, see [plug_server_timing](https://github.com/scoutapp/elixir_plug_server_timing) on GitHub.
