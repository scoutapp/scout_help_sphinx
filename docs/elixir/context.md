# Scout Elixir Custom Context

[Context](#context) lets you see the forest from the trees. For example, you can add custom context to answer critical questions like:

* How many users are impacted by slow requests?
* How many trial customers are impacted by slow requests?
* How much of an impact are slow requests having on our highest paying customers?

It's simple to add [custom context](#context) to your app. There are two types of context:

## User Context

For context used to identify users (ex: email, name):

```elixir
ScoutApm.add_user(key, value)
```

Examples:

```elixir
ScoutApm.Context.add_user(:email, user.email)
ScoutApm.Context.add_user(:name, user.name)
```

## General Context

```elixir
ScoutApm.Context.add(key, value)
```

Examples:

```elixir
ScoutApm.Context.add(:account, account.name)
ScoutApm.Context.add(:monthly_spend, account.monthly_spend)
```

## Default Context

Scout reports the Request URI and the user's remote IP Address by default.

## Context Value Types

Context values can be any of the following types:

* Printable strings (`String/printable?/1` returns `true`)
* Boolean
* Number

## Context Key Restrictions

Context keys can be an `Atom` or `String` with only printable characters. Custom context keys may contain alphanumeric characters, dashes, and underscores. Spaces are not allowed.

Attempts to add invalid context will be ignored.
