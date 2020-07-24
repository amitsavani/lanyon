---
layout: post
title: "How to fix `DEPRECATION WARNING: Dangerous query method` on `pluck`?"
article: 3
---
Suddenly I noticed a peculiar deprecation warning on `.pluck` in log everywhere.

```
DEPRECATION WARNING: Dangerous query method (method whose arguments are used as raw SQL) called with non-attribute argument(s): [:client_id, :duns, :legal_business_name]. Non-attribute arguments will be disallowed in Rails 6.1. This method should not be called with user-provided values, such as request parameters or model attributes. Known-safe values can be passed by wrapping them in Arel.sql()
```

Here is a simplified snippet which was generating the warning:

```ruby
ERRORED_RECORDS_FIELDS = %i(client_id duns legal_business_name)
...
...
Client.pluck(ERRORED_RECORDS_FIELDS)
```

However, the result returns just fine. Reading a bit around the warning I relized that I have been passing array if fields which at first point looks ok but it's not.

So instead of passing array, splat the array so that all elements are received as separate column value in the `args` of the `pluck` method.

So following fixes the issue.

```ruby
Client.pluck(*ERRORED_RECORDS_FIELDS)
```

Thanks!