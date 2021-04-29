---
layout: post
title: "TIL - Use Struct over OpenStruct for performance"
article: 8
---
TL;DR `Struct` is much faster than `OpenStruct`.

I [learned this](https://gitlab.com/gitlab-org/gitlab-styles/-/merge_requests/82) while [contributing to Gitlab](https://gitlab.com/gitlab-org/gitlab/-/merge_requests?scope=all&utf8=%E2%9C%93&state=opened&author_username=amit.savani).

There has already been [written](https://www.rubyguides.com/2017/06/ruby-struct-and-openstruct/) about  `Struct` and `OpenStruct`. But this post focuses on the performance implications of using one over other.

Developers have tendency to use `OpenStruct` over `Struct` generally because with  `OpenStruct`, we can arbitrarily set & access attributes. However, in case of `Struct`, we can only set & access attributes defined at the time of  `Struct` declaration.

e.g.,

<script src="https://gist.github.com/amitsavani/c030ed4d908caed96c69c7b8560e852f.js"></script>

Here is a quick benchmark report performed on MacOS taken from [Gabriel Mazetto's comment on Gitlab](https://gitlab.com/gitlab-org/gitlab-styles/-/merge_requests/82#note_539079285) for quick reference:

#### Ruby 2.7.2

```
Warming up --------------------------------------
      openstruct new   138.338k i/100ms
   openstruct access     6.255k i/100ms
          struct new   300.884k i/100ms
      struct acccess   240.079k i/100ms
Calculating -------------------------------------
      openstruct new      1.409M (± 2.1%) i/s -      7.055M in   5.008396s
   openstruct access     62.552k (± 3.6%) i/s -    312.750k in   5.007322s
          struct new      3.032M (± 1.8%) i/s -     15.345M in   5.063488s
      struct acccess      2.397M (± 1.4%) i/s -     12.004M in   5.008534s

Comparison:
          struct new:  3031505.6 i/s
      struct acccess:  2397159.8 i/s - 1.26x  (± 0.00) slower
      openstruct new:  1409319.1 i/s - 2.15x  (± 0.00) slower
   openstruct access:    62551.7 i/s - 48.46x  (± 0.00) slower
```

#### Ruby 3.0.0

```
Warming up --------------------------------------
      openstruct new     5.654k i/100ms
   openstruct access     4.880k i/100ms
          struct new   298.290k i/100ms
      struct acccess   237.080k i/100ms
Calculating -------------------------------------
      openstruct new     57.811k (± 1.7%) i/s -    294.008k in   5.087150s
   openstruct access     49.124k (± 2.1%) i/s -    248.880k in   5.068713s
          struct new      2.972M (± 1.7%) i/s -     14.914M in   5.020725s
      struct acccess      2.363M (± 3.0%) i/s -     11.854M in   5.022310s

Comparison:
          struct new:  2971520.7 i/s
      struct acccess:  2362714.6 i/s - 1.26x  (± 0.00) slower
      openstruct new:    57811.0 i/s - 51.40x  (± 0.00) slower
   openstruct access:    49123.9 i/s - 60.49x  (± 0.00) slower
```
You can see here `OpenStuct` is **60 times slower** than `Struct`.

Even `OpenStuct` can be slower than `Hash` in some cases. From the [Ruby doc](https://github.com/ruby/ruby/blob/7e93917458cdc67399e82233ff0f13e3c8bd7065/lib/ostruct.rb#L76-L77),

> Creating an open struct from a small Hash and accessing a few of the entries can be 200 times slower than accessing the hash directly.

Go for `Struct` whenever possible over `OpenStruct`.
