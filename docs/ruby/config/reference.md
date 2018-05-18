## Scout Ruby Configuration Reference

The following configuration settings are available:

<table class="lookup">
  <thead>
    <tr>
      <th>
        Setting&nbsp;Name
      </th>
      <th>
        Description
      </th>
      <th>
        Default
      </th>
      <th>
        Required
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>
        name
      </th>
      <td>
        Name of the application (ex: 'Photos App').
      </td>
      <td>
        <code>Rails.application.class.to_s.
           sub(/::Application$/, '')</code>
      </td>
      <td>
        Yes
      </td>
    </tr>
    <tr>
      <th>
        key
      </th>
      <td>
        The organization API key.
      </td>
      <td></td>
      <td>
        Yes
      </td>
    </tr>
    <tr>
      <th>
        monitor
      </th>
      <td>
        Whether monitoring should be enabled.
      </td>
      <td>
        <code>false</code>
      </td>
      <td>
        No
      </td>
    </tr>
    <tr>
      <th>
        log_level
      </th>
      <td>
        The logging level of the agent.
      </td>
      <td>
        <code>INFO</code>
      </td>
      <td>
        No
      </td>
    </tr>
    <tr>
      <th>
        log_file_path
      </th>
      <td>The path to the <code>scout_apm.log</code> log file directory. Use <code>stdout</code> to log to STDOUT.
      </td>
      <td>
        <code>Environment#root+log/</code>&nbsp;or <code>STDOUT</code> if running on Heroku.
      </td>
      <td>
        No
      </td>
    </tr>
    <tr>
      <th>
        hostname
      </th>
      <td>
        The hostname the metrics should be aggregrated under.
      </td>
      <td>
        <code>Socket.gethostname</code>
      </td>
      <td>
        No
      </td>
    </tr>
    <tr>
      <th>
        proxy
      </th>
      <td>Specify the proxy URL (ex: <code>https://proxy</code>) if a proxy is required.
      </td>
      <td></td>
      <td>
        No
      </td>
    </tr>
    <tr>
      <th>
        host
      </th>
      <td>
        The protocol + domain where the agent should report.
      </td>
      <td>
        <code>https://apm.scoutapp.com</code>
      </td>
      <td>
        No
      </td>
    </tr>
    <tr>
      <th>
        uri_reporting
      </th>
      <td>
        By default Scout reports the URL and filtered query parameters with transaction traces. Sensitive parameters in the URL will be redacted. To exclude query params entirely, use
        <code>path</code>.
      </td>
      <td>
        <code>filtered_params</code>
      </td>
      <td>
        No
      </td>
    </tr>
    <tr>
      <th>
        disabled_instruments
      </th>
      <td>
        An Array of instruments that Scout should not install. Each Array element should should be a string-ified, case-sensitive class name (ex: <code>['Elasticsearch','HttpClient']</code>). The default installed instruments can be viewed in the <a href="https://github.com/scoutapp/scout_apm_ruby/tree/master/lib/scout_apm/instruments" target="_blank">agent source</a>.
      </td>
      <td>
        <code>[]</code>
      </td>
      <td>
        No
      </td>
    </tr>
    <tr>
      <th>
        ignore
      </th>
      <td>
        An Array of web endpoints that Scout should not instrument. Routes that match the prefixed path (ex: <code>['/health', '/status']</code>) will be ignored by the agent.
      </td>
      <td>
        <code>[]</code>
      </td>
      <td>
        No
      </td>
    </tr>
    <tr>
      <th>
        enable_background_jobs
      </th>
      <td>
        Indicates if background jobs should be monitored.
      </td>
      <td>
        <code>true</code>
      </td>
      <td>
        No
      </td>
    </tr>
    <tr>
      <th>dev_trace</th>
      <td>
        Indicates if DevTrace, the Scout development profiler, should be enabled. Note this setting only applies
        to the development environment.
      </td>
      <td>
        <code>false</code>
      </td>
      <td>No</td>
    </tr>
    <tr>
      <th>profile</th>
      <td>
        Indicates if ScoutProf, the Scout code profiler, should be enabled.
      </td>
      <td>
        <code>true</code>
      </td>
      <td>No</td>
    </tr>
    <tr>
      <th>revision_sha</th>
      <td>
        The Git SHA that corresponds to the version of the app being deployed.
      </td>
      <td>
        <a href="#ruby-deploy-tracking-config">See docs</a>
      </td>
      <td>No</td>
    </tr>
    <tr>
      <th>detailed_middleware</th>
      <td>
        When true, the time spent in each middleware is visible in transaction traces vs. an aggregrate across all middlewares. This
        adds additional overhead and is disabled by default as middleware is an uncommon bottleneck.
      </td>
      <td>
        <code>false</code>
      </td>
      <td>No</td>
    </tr>
  </tbody>
</table>