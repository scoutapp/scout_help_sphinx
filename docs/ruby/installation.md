# Scout Ruby Installation

Tailored instructions are provided within our user interface. General instructions:

<table class="help install install_ruby">
  <tbody>
    <tr>
      <td>
        <span class="step">1</span>
      </td>
      <td>
        <p>Your Gemfile:</p>
<pre>
gem 'scout_apm'
</pre>

<p><strong>Shell:</strong></p>

<pre>
bundle install
</pre>
      </td>
    </tr>
    <tr>
      <td><span class="step">2</span></td>
      <td><p>Download your customized config file*, placing it at <code>config/scout_apm.yml</code>.</p>
        <p class="smaller">Your customized config file is available within your Scout account.</p>
      </td>
    </tr>
    <tr>
      <td><span class="step">3</span></td>
      <td><p>Deploy.</p></td>
    </tr>
  </tbody>
</table>

<p>
* - If you've installed Scout via the Heroku Addon, the provisioning process automatically sets the required settings via <a href="https://devcenter.heroku.com/articles/config-vars">config vars</a> and a configuration file isn't required.
</p>