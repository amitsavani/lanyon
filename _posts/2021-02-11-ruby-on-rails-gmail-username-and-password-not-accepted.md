---
layout: post
title: "Ruby on Rails, Gmail and Net::SMTPAuthenticationError: 535-5.7.8 Username and Password not accepted"
article: 7
---

Are you getting following error while using Gmail SMTP?


```ruby
Net::SMTPAuthenticationError: 535-5.7.8 Username and Password not accepted
```

The solution is simple.

1. Go to https://www.google.com/settings/security/lesssecureapps
2. Enable the switch marked below and you are done!

[![Allow Less Secure Apps](/public/images/a-007-1.png)](/public/images/a-007-1.png)
