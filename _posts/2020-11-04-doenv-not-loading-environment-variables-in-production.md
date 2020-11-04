---
layout: post
title: "Dotenv : Are the environment variables not loading in the `production` environment in your Rails application?"
article: 6
---

You have configured your `.env` for all common environment variables across environments but they are not loading in `production` environment.

Here is a catch in the intallation doc.

[![could not fetch specs from https://rubygems.org/](/public/images/a-006-1.png)](/public/images/a-006-1.png)

Here you can see that the document mentions to install the gem only in `development` and `test` environments and you just copied and pasted as is in your `Gemfile` !


The fix is simple. Just remove `groups: [:development, :test]` or put the `dotenv-rails` gem outside of all environment like ohter gems required in all environment.

