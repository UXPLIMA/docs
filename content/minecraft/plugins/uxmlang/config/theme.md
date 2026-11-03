---
title: theme.conf
order: 607
description: The colours, roles, wheel, gradients, glyphs, prefixes and small capitals every UXM plugin shares.
icon: palette
---

`theme.conf` is the look of the server: which colour each role is, the glyphs menus are drawn with, and which
languages are written in small capitals. It sits in `plugins/uxmTheme/`, beside the plugin folders, and every UXM
plugin reads the same file, so one edit changes the whole suite. The first UXM plugin installed writes it; an
existing file is never touched.

A plugin's own `theme.conf` in its own folder, `plugins/uxmLang/theme.conf`, is read on top of the shared one
key by key, for a server that wants this plugin to differ. A key left out anywhere keeps the shipped value, so a
three-line file is valid. `/uxmlang reload` reads it again. A file that cannot be read is reported in the console
and the shipped look is used.

## The three layers

| Layer | Holds | Who names it |
|---|---|---|
| `palette` | The colours, under names you choose | Only this file |
| `roles` | The job of each colour | The messages files and item lines |
| `wheel` | The colours decoration is taken from, in order | The windows of the plugins that have them |

A message never names a colour, only a role, so renaming or replacing a palette colour breaks nothing.

## palette

| Name | Colour | Name | Colour |
|---|---|---|---|
| `rose` | `#FF6B8B` | `white` | `#FFFFFF` |
| `peach` | `#FFA07A` | `mist` | `#DDE8F0` |
| `butter` | `#FFE66D` | `steel` | `#9AA5BE` |
| `mint` | `#4ECCA3` | `stone` | `#8A93A1` |
| `sky` | `#48CAE4` | `slate` | `#6B7886` |
| `corn` | `#6C8DFB` | `ash` | `#565F6B` |
| `lilac` | `#B388FF` | `cyan` | `#4FD6E8` |

## roles

| Role | Ships as | Used for |
|---|---|---|
| `accent` | `sky` | The prefix, headers, a menu's accent |
| `body` | `white` | The text of a message |
| `subtext` | `mist` | A description in a menu |
| `muted` | `steel` | A counter or a date |
| `dim` | `slate` | A separator, something that cannot be used |
| `icon` | `stone` | The glyph at the start of a tooltip line |
| `crumb` | `ash` | The category line under a tooltip title |
| `value` | `butter` | A name, a number, a duration |
| `good` | `mint` | Success |
| `bad` | `rose` | An error, a refusal |
| `warn` | `peach` | A warning |
| `money` | `butter` | An amount of money |
| `level` | `lilac` | A level |
| `cta` | `butter` | The word "click" |
| `info` | `cyan` | A neutral note |
| `rank` | `lilac` | A rank |
| `event` | `corn` | An event or a bonus |

A value is a palette name or a hex code. The list is open: a role you add here can be used as a tag in your own
lines.

## wheel and gradients

`wheel = [ rose, peach, butter, mint, sky, corn, lilac ]` is the order decoration takes its colours in; it wraps, so a
long menu keeps working. `gradients.header = [ butter ]` paints every `<h:'...'>` header; write two or more colours for
a heading that sweeps.

## glyphs

| Glyph | Key | Where |
|---|---|---|
| `▶` | `separator` | Between a chat line's category word and the sentence |
| `◆` | `title` | The first line of a tooltip |
| `✎` | `description` | The description header of a tooltip |
| `≡` | `details` | The details header |
| `•` | `row`, `status` | One fact, or one state, under the details header |
| `→` | `action` | The line that says what a click does |

## prefix

`prefix.categories` gives a category word its colour by role: `error = "bad"`, `money = "good"`, `shop = "good"`,
`event = "event"`. A category not named reads in `accent`, as uxmLang's `LANGUAGE` does.

## small capitals

```hocon
small-caps { en = true, tr = true, de = true, es = true, fr = true, pt = true }
```

The languages written in small capitals. They exist for the 26 Latin letters only; any other letter keeps its shape,
so Turkish keeps `ç`, `ğ`, `ı`, `ö`, `ş` and `ü`. A language whose writing is not Latin, such as Russian, has nothing
to convert. Values, numbers and commands stay as written.

Related: [Messages](messages.md), [Languages](languages.md)
