# Scout Ruby DevTrace

To enable [DevTrace](#devtrace), our in-browser profiler:

<table class="help install">
  <tbody>
    <tr>
      <td>
        <span class="step">1</span>
      </td>
      <td>
        <p>Ensure you are on the latest version of <code>scout_apm</code>.
          See our <a href="#updating-to-the-newest-version">upgrade instructions</a>.
        </p>
      </td>
    </tr>
    <tr>
      <td><span class="step">2</span></td>
      <td>
        <p style="line-height: 170%">
          Add <code>dev_trace: true</code> to the <code>development</code> section of your <code>scout_apm.yml</code> config file or start your local Rails server with:
          <code>SCOUT_DEV_TRACE=true rails server</code>.
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">3</span>
      </td>
      <td>
        <p>Refresh your browser window and look for the speed badge.</p>
      </td>
    </tr>
  </tbody>
</table>
