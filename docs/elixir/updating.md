# Updating to the newest Scout Elixir Version

<table class="help">
  <tbody>
    <tr>
      <td>
        <span class="step">1</span>
      </td>
      <td>
<p>Ensure your <code>mix.exs</code> dependency entry for <code>scout_apm</code> is: <code>{:scout_apm, "~> 0.0"}`</code></p>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">2</span>
      </td>
      <td>
<p> Inside your project root:</p>

```
mix deps.get scout_apm
```

</td>
</tr>
<tr>
<td>
  <span class="step">3</span>
</td>
<td>
<p>Recompile and deploy.</p>
</td>
    </tr>
  </tbody>
</table>