# Scout Ruby Custom Instrumentation

Traces that allocate significant amount of time to `Controller` or `Job` are good candidates to add custom instrumentation. This indicates a significant amount of time is falling outside our default instrumentation.

## Limits

We limit the number of metrics that can be instrumented. Tracking too many unique metrics can impact the performance of our UI. Do not dynamically generate metric types in your instrumentation (ie `self.class.instrument("user_#{user.id}", "generate") { ... })` as this can quickly exceed our rate limits.

## Instrumenting method calls

To instrument a method call, add the following to the class containing the method:

```ruby
  class User
    include ScoutApm::Tracer

    def export_activity
      # Do export work
    end
    instrument_method :export_activity
  end
```

The call to `instrument_method` should be after the method definition.

### Naming methods instrumented via `instrument_method`

In the example above, the metric will appear in traces as `User#export_activity`. On timeseries charts, the time will be allocated to a `Custom` type.

__To modify the type__:

```ruby
instrument_method :export_activity, type: 'Exporting'
```

A new `Exporting` metric will now appear on charts. The trace item will be displayed as `Exporting/User/export_activity`.

__To modify the name__:

```ruby
instrument_method :export_activity, type: 'Exporting', name: 'user_activity'
```

The trace item will now be displayed as `Exporting/user_activity`.

### Instrumenting blocks of code

To instrument a method call, add the following:

```ruby
  class User
    include ScoutApm::Tracer

    def generate_profile_pic
      self.class.instrument("User", "generate_profile_pic") do
        # Do work
      end
    end
  end
```

### Naming methods instrumented via `instrument(type, name)`

In the example above, the metric appear in traces as `User/generate_profile_pic`. On timeseries charts, the time will be allocated to a `User` type. To modify the type or simply, simply change the `instrument` corresponding method arguments.

<h2 id="ruby-renaming-transactions">Renaming transactions</h2>

There may be cases where you require more control over how Scout names transactions. For example, if you have a controller-action that renders both JSON and HTML formats and the rendering time varies significantly between the two, it may make sense to define a unique transaction name for each.

Use `ScoutApm::Transaction#rename`:

```ruby
class PostsController < ApplicationController
  def index                              
    ScoutApm::Transaction.rename("posts/foobar")                                   
    @posts = Post.all                    
  end
end
```

In the example above, the default name for the transaction is `posts/index`, which appears as `PostsController#index` in the Scout UI. Renaming the transaction to `posts/foobar` identifies the transaction as `PostsController#foobar` in the Scout UI.  

Do not generate highly cardinality transaction names (ex: `ScoutApm::Transaction.rename("posts/foobar_#{current_user.id}")`) as we limit the number of transactions that can be tracked. High-cardinality transaction names can quickly surpass this limit.

<h2 id="ruby-testing-instrumentation">Testing instrumentation</h2>

Improper instrumentation can break your application. It's important to test before deploying to production. The easiest way to validate your instrumentation is by running [DevTrace](#devtrace) and ensuring the new metric appears as desired.

After restarting your dev server with DevTrace enabled, refresh the browser page and view the trace output. The new metric should appear in the trace:

![custom devtrace](images/custom_devtrace.png)