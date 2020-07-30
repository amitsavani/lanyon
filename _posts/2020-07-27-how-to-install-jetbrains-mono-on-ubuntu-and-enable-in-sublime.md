---
layout: post
title: How to install JetBrains Mono on Ubuntu and enable in Sublime?
article: 4
---
JetBrains recently released [JetBrains Mono fonts](https://www.jetbrains.com/lp/mono/) for free.

It is very friendly to developers as the font has,

- Increased height and letter spacing for a better reading experience(study says we spend 80%+ time on reading code)

[![without ligatures](/public/images/a-004-4.gif)](/public/images/a-004-4.gif)

- Code-specific ligatures merges multiple symbols helps eyes to process less

*Without Ligature*
[![without ligatures](/public/images/a-004-1.png)](/public/images/a-004-1.png)

*With Ligature* ❤
[![without ligatures](/public/images/a-004-2.png)](/public/images/a-004-2.png)

- And it's free & open source

## How install in Ubuntu?

### Download and Install

##### Option 1

Use browser to [download zip from Jetbrains Mono](https://www.jetbrains.com/lp/mono/) page and `unzip` it in `~/.local/share/fonts/` to use it for specific user or `/usr/share/fonts` for system wide use.

##### Option 2

Use command line

```
$ wget https://download.jetbrains.com/fonts/JetBrainsMono-2.001.zip
$ unzip JetBrainsMono-2.001.zip
```

To use it for specific user:

```
$ mv JetBrainsMono-*.ttf ~/.local/share/fonts/
```

To use it sysem-wide:

```
$ sudo mv JetBrainsMono-*.ttf /usr/share/fonts/
```


### How to enable in Sublime Text?

Download latest version from [here](https://www.jetbrains.com/lp/mono/)

In sublime,

1. Open `Preferences > Settings`
2. Add `"font_face": "JetBrains Mono"` on the User's pref
3. Save (`Ctrl+S`)

[![without ligatures](/public/images/a-004-3.png)](/public/images/a-004-3.png)

Thant's it!