# Scout Ruby Custom Context

[Context](#context) lets you see the forest from the trees. For example, you can add custom context to answer critical questions like:

* How many users are impacted by slow requests?
* How many trial customers are impacted by slow requests?
* How much of an impact are slow requests having on our highest paying customers?

It's simple to add [custom context](#context) to your app. There are two types of context:

## User Context

For context used to identify users (ex: email, name):

```ruby
ScoutApm::Context.add_user({})
```

Examples:

```ruby
ScoutApm::Context.add_user(email: @user.email)
ScoutApm::Context.add_user(email: @user.email, location: @user.location.to_s)
```

## General Context

```ruby
ScoutApm::Context.add({})
```

Examples:

```ruby
ScoutApm::Context.add(account: @account.name)
ScoutApm::Context.add(database_shard: @db.shard_id, monthly_spend: @account.monthly_spend)
```

## Default Context

Scout reports the Request URI and the user's remote IP Address by default.

## Context Types

Context values can be any of the following types:

* Numeric
* String
* Boolean
* Time
* Date

## Context Field Name Restrictions

Custom context names may contain alphanumeric characters, dashes, and underscores. Spaces are not allowed.

Attempts to add invalid context will be ignored.

## Example: adding the current user's email as context

Add the following to your `ApplicationController` class:

```ruby
before_filter :set_scout_context
```

Create the following method:

```ruby
def set_scout_context
  ScoutApm::Context.add_user(email: current_user.email) if current_user.is_a?(User)
end
```

## Example: adding the monthly spend as context

Add the following line to the `ApplicationController#set_scout_context` method defined above:

```ruby
ScoutApm::Context.add(monthly_spend: current_org.monthly_spend) if current_org
```