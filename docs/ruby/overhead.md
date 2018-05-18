# Scout Ruby Overhead Information

Scout is built for production monitoring and is engineered to add minimal overhead. We test against several open-source benchmarks on significant releases to prevent releasing performance regressions. 

There are a couple of scenarios worth mentioning where more overhead than expected may be observed. 

## Enabling the detailed_middleware option

By default, Scout aggregates all middleware timings together into a single "Middleware" category. Scout can provide a detailed breakdown of middleware timings by setting `detailed_middleware: true` in the configuration settings.

This is `false` by default as instrumenting each piece of middleware adds additional overhead. It's common for Rails apps to use more than a dozen pieces of middleware. Typically, time spent in middleware is very small and isn't worth instrumenting. Additionally, most of these middleware pieces are maintained by third-parties and are thus more difficult to optimize.

## Resque Instrumentation

Since Resque works by forking a child process to run each job and exiting immediately when the job is finished, our instrumentation needs a way to aggregate the timing results and traces into a central store before reporting the information to our service. To support Resque, the Resque child process sends a simple payload to the parent which is listening via WEBRick on localhost. As long as there is one WEBRick instance listening on the configured port, then any Resque children will be able to send results back to it.

The overhead is usually small, but it is more significant than instrumenting background job frameworks like Sidekiq and DelayedJob that do not use forking. The lighter the jobs are, more overhead is incurred in the serialization and reporting to WEBRick. In our testing, for jobs that took ~18 ms each, we found that the overhead is about ~8%. If your jobs take longer than that, on average, the overhead will be lower.
