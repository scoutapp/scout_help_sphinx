# Scout Elixir DevTrace

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
        
```
# config/dev.exs
config :scout_apm,
  dev_trace: true
```
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