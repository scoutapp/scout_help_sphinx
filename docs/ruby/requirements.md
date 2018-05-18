# Scout Ruby Requirements

Our agent supports Ruby 1.8.7+.

* Web frameworks
  * Ruby on Rails 2.2+
  * Rack
  * Sinatra
* Background Job libraries
  * Sidekiq
  * DelayedJob
  * Resque

All Ruby web app servers are supported (Puma, Passenger, Unicorn, WEBrick, Thin, etc.).

[Memory Bloat detection](#memory-bloat-detection) and [ScoutProf](#scoutprof) require Ruby 2.1+.