# Scout Ruby Troubleshooting

## No Data

Not seeing any data?

<table class="help">
  <tbody>
    <tr>
      <td>
        <span class="step">1</span>
      </td>
      <td>
        <p>Is there a <code>log/scout_apm.log</code> file?</p>
        <p><strong style="color:gray">Yes:</strong></p>
        <p>Examine the log file for error messages:</p>
<pre>
tail -n1000 log/scout_apm.log | grep "Starting monitoring" -A20
</pre>
        <p>
          See something noteworthy? Proceed to <a href="#step7">to the last step</a>. Otherwise, continue to step 2.
        </p>
        <p><strong style="color:gray">No:</strong></p>
        <p>
          The gem was never initialized by the application.</p>
        <p>
          Ensure that the <code>scout_apm</code> gem is not restricted to a specific <code>group</code> in your <code>Gemfile</code>. For example, the configuration below would prevent <code>scout_apm</code> from loading in a <code>staging</code> environment:
        </p>
<pre>
group :production do
  gem 'unicorn'
  gem 'scout_apm'
end
</pre>
        <p><a href="#step7">Jump to the last step</a> if <code>scout_apm</code> is correctly configured in your <code>Gemfile</code>.</p>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">2</span>
      </td>
      <td>
        <p>Was the <code><strong>scout_apm</strong></code> gem deployed with your application?</p>
<pre>
bundle list scout_apm
</pre>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">3</span>
      </td>
      <td>
        <p>Did you download the config file, placing it in <code><strong>config/scout_apm.yml</strong></code>?</p>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">4</span>
      </td>
      <td>
        <p>
          Did you restart the app?
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">5</span>
      </td>
      <td>
        <a name="step5"></a>
        <p>Are you sure the application has processed any requests?</p>
<pre>
tail -n1000 log/production.log | grep "Processing"
</pre>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">6</span>
      </td>
      <td>
        <p>
          Using Unicorn?
        </p>
        <p>Add the <code>preload_app true</code> directive to your Unicorn config file. <a href="http://unicorn.bogomips.org/Unicorn/Configurator.html#method-i-preload_app">Read more</a> in the Unicorn docs.
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <a name="step7"></a>
        <span class="step">7</span>
      </td>
      <td>
        <p>
          Oops! Looks like messed up. Check out the <a href="https://github.com/scoutapp/scout_apm_ruby/issues">GitHub issues</a> and <a href="mailto:support@scoutapp.com">send us an email</a> with the following:
        </p>
        <ul>
          <li>The last 1000 lines of your <code>log/scout_apm.log</code> file, if the file exists:<br/><code>tail -n1000 log/scout_apm.log</code>.
          </li>
          <li>Your application's gems <code>bundle list</code>.
          </li>
          <li>
            Rails version
          </li>
          <li>
            Application Server (examples: Passenger, Thin, etc.)
          </li>
          <li>
            Web Server (examples: Apache, Nginx, etc.)
          </li>
        </ul>
        <p>
          We typically respond within a couple of hours during the business day.
        </p>
      </td>
    </tr>
  </tbody>
</table>

## Significant time spent in "Controller" or "Job"

When viewing a transaction trace, you may see time spent in the "controller" or "job" layers. This is time that falls outside of Scout's default instrumentation. There are two options for gathering additional instrumentation:

1. [Custom Instrumentation](#ruby-custom-instrumentation) - use our API to instrument pieces of code that are potential bottlenecks.
2. [ScoutProf](#scoutprof) - install our BETA agent which adds ScoutProf. ScoutProf breaks down time spent within the controller layer. Note that ScoutProf does not instrument background jobs.

## Missing memory metrics

Memory allocation metrics require the following:

* Ruby version 2.1+
* `scout_apm` version 2.0+

If the above requirements are not met, Scout continues to function but does not report allocation-related metrics.