---
title: languages.conf
order: 603
description: The name each language is written with, and the icon the window draws it with.
icon: list
---

A language exists because `messages/` holds a file for it. This file only names it, so a window and a message can
show `Türkçe` rather than `tr`, and gives it an icon. Read at start and on every reload.

## names

```hocon
names {
  en = "English"
  tr = "Türkçe"
  de = "Deutsch"
  es = "Español"
  fr = "Français"
  pt = "Português"
  ru = "Русский"
}
```

A language nobody names here is written by the server in its own words, which is right often enough.

## icons

```hocon
icons {
  default = "PAPER"
  en = "basehead:eyJ0ZXh0dXJlcyI6..."
}
```

An icon is a material such as `PAPER`, or a head written as `basehead:<value>`, the long Value a head site lists under
"For Developers". A language with no icon takes `default`. The shipped flags are the country each file is written for:

| Language | Flag |
|---|---|
| `en` | The United Kingdom |
| `tr` | Turkey |
| `de` | Germany |
| `es` | Spain |
| `fr` | France |
| `pt` | Brazil, as the Portuguese file is Brazilian |
| `ru` | Russia |

Related: [Language window](../menus/language.md), [Languages](languages.md)
