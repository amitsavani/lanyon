---
layout: post
title: Digging into `could not fetch specs from https://rubygems.org/` error
article: 1
---
[![could not fetch specs from https://rubygems.org/](/public/images/a-001-1.png)](/public/images/a-001-1.png)

I suddenly started facing this issue one fine morning when I tried to use a well maintained gem in a rails app. There was absolutely nothing changed per say anywhere in the codebase since I did last commit. The build passed on CI server.

I tried stumbling around for a solution but couldn't figure out. I tried couple of solutions one by one mentioned below form StackOverflow, Github issues and other forums:

1. Replace `https` with `http` in Gemfile source URL
2. Try to update gem  `gem update --system`
3. Updated RVM to latest
4. Run `ruby -ropen-uri -e 'eval open("https://git.io/vQhWq").read` as per RVM doc to troubleshoot the issue but end up with following error

[![could not fetch specs from https://rubygems.org/](/public/images/a-001-2.png)](/public/images/a-001-2.png)


I tried it with different apps on my machine with different ruby and rails versions but same error.

Suddenly there is a message from inside that there must be problem with the internet. That unfortunate day my broadband network was down. I was using a hotspot from my mobile. I tried with my wife’s mobile as well but no luck. Both were using same service provider.

In the evening after spending hours, I managed to get the broadband connection back. I tried to install gem again and voila!!! The problem absolutely did not exist.

So **first check your internet connection** before looking into other solutions.
