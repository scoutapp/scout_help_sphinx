# Scout Elixir Config Reference

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
      </td>
      <td>
        Yes
      </td>
    </tr>
    <tr>
      <th>
        monitor
      </th>
      <td>
        Whether monitoring data should be reported.
      </td>
      <td>
        <code>true</code>
      </td>
      <td>
        No
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
        dev_trace
      </th>
      <td>
        Indicates if DevTrace, the Scout development profiler, should be enabled.
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
        log_level
      </th>
      <td>
        The logging level of the agent. Possible values: <code>:debug</code>, <code>:info</code>, <code>:warn</code>, and <code>:error</code>.
      </td>
      <td>
        <code>:info</code>
      </td>
      <td>
        No
      </td>
    </tr>
  </tbody>
</table>