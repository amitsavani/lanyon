---
layout: post
title: "CanCanCan : Use differnt column to `load_and_authorize_resource` in inherited controllers"
article: 5
---

Recently I came across a situation where there was a need to use differnt column for `load_and_authorize_resource` in an inherited controller.

By default [CanCanCan](https://github.com/CanCanCommunity/cancancan), use `id` to find resources in `load_and_authorize_resource`

If you want to use different column, you can use `:find_by` option:

```ruby
load_and_authorize_resource :find_by => :slug
```

But this will apply on all the controller inheriting it.

```ruby
# app/controllers/application_controller.rb

class ApplicationController < ActionController::Base

  load_and_authorize_resource

end
```

```ruby
# app/controllers/posts_controller.rb
class PostsController < ApplicationController

  # for this controller, resource will be load using `id` 

end
```

If you try to put `load_and_authorize_resource :find_by => :slug` in `posts_controller.rb` hoping it will override the behaviour then you are wrong. It doesn't work.

To make it working, you need to use `:prepend` option.


```ruby
# app/controllers/posts_controller.rb
class PostsController < ApplicationController

  load_and_authorize_resource :find_by => :slug, prepend: true

end
```

Hope this helps!
