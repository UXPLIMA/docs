---
title: PlaceholderAPI
order: 903
description: The uxmlevels expansion, and placeholders from other plugins inside this one.
icon: braces
---

With PlaceholderAPI installed, uxmLevels registers the `uxmlevels` expansion at start, and takes it down again when
the plugin stops. Nothing needs downloading with `/papi ecloud`.

| You want | Use |
|---|---|
| A level on a scoreboard | `%uxmlevels_<track>_level%` |
| A badge in chat | `%uxmlevels_<track>_badge%` |
| A board on a hologram | `%uxmlevels_top_<track>_<period>_<n>_name%` and the rest |

Every key is on the [reference](../placeholders/reference.md). A reward line's `{if=...}` condition may name any
placeholder of another plugin. Without PlaceholderAPI, the plugin runs fully and `/uxmlevels doctor` warns.

Related: [Placeholders](../placeholders/reference.md)
