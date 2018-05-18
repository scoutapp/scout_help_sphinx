## Ignoring Scout Ruby transactions

There are a couple of approaches to ignore web requests and background jobs you don't care to instrument. These approaches are listed below.

### By the web endpoint path name

You can ignore requests to web endpoints that match specific paths (like `/health_check`). See the `ignore` setting in the [configuration options](#ruby-configuration-options).

### In your code

To selectively ignore a web request or background job in your code, add the following within the transaction:

```ruby
ScoutApm::Transaction.ignore!
```

### Ignoring all background jobs

You can ignore all background jobs by setting `enable_background_jobs: false` in your configuration file. See the [configuration options](#ruby-configuration-options).