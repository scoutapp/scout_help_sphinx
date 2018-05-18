# Scout Elixir Troubleshooting

Not seeing data?

<table class="help">
  <tbody>
    <tr>
      <td>
        <span class="step">1</span>
      </td><td>
        <p>Examine your log file for any lines that match <code>Scout</code>.</p>

<p>Look for:</p>

```
[info] Setup ScoutApm.Watcher on ScoutApm.Store
[info] Setup ScoutApm.Watcher on ScoutApm.Config
[info] Setup ScoutApm.Watcher on ScoutApm.PersistentHistogram
[info] Setup ScoutApm.Watcher on ScoutApm.Logger
[info] Setup ScoutApm.Watcher on ScoutApm.Supervisor
```

<p>
  If none of the above appears, ensure <code>scout_apm</code> was added as a dependency and to your list of applications. See the first step in the <a href="#elixir-install">Elixir install instructions</a>.
</p>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">2</span>
      </td>
      <td>
        <p>Is <code>use ScoutApm.Instrumentation</code> specified in <em>every</em> controller module you wish to instrument?</p>

<p>
  This step is frequently missed if you are using multiple controller modules. See the third step in the <a href="#elixir-install">Elixir install instructions</a>.
</p>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">3</span>
      </td>
      <td>
        <p>Still stuck? Email us.</p>

The following process helps us resolve issues faster:
 <ul>
    <li>Increase the log level of <code>scout_apm</code> by setting <code>log_level: "debug"</code> in your <code>config/scout_apm.exs</code> file and restart your app.</li>
    <li>
      Wait five minutes, then email <a href="mailto:support@scoutapp.com">support@scoutapp.com</a> 
      your log output and the application's <code>mix.lock</code> file.</li>
  </ul>

<p>We typically respond within a couple of hours during the business day.</p>
      </td>
    </tr>
  </tbody>
</table>
