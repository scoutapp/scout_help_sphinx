## Sampling Scout Ruby web requests

Use probability sampling to limit the number of web requests Scout analyzes:

```ruby
# app/controllers/application_controller.rb
before_action :sample_requests_for_scout

def sample_requests_for_scout
  # Sample rate should range from 0-1:
  # * 0: captures no requests
  # * 0.75: captures 75% of requests
  # * 1: captures all requests
  sample_rate = 0.75

  if rand > sample_rate
    Rails.logger.debug("[Scout] Ignoring request: #{request.original_url}")
    ScoutApm::Transaction.ignore!
  end
end
```