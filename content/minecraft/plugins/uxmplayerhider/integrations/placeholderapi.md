---
title: PlaceholderAPI
order: 702
description: The uxmplayerhider expansion, and placeholders from other plugins inside this one.
icon: braces
---

With PlaceholderAPI installed, uxmPlayerHider registers the `uxmplayerhider` expansion at start, and takes it down
again when the plugin stops. Nothing needs downloading with `/papi ecloud`; the expansion is inside the plugin.

| You want | Use |
|---|---|
| The state on a scoreboard | `%uxmplayerhider_state%` |
| How many players a player hides | `%uxmplayerhider_hidden%` |
| A line only for players who hide someone | `%uxmplayerhider_hiding%` |

Every key is on the [reference](../placeholders/reference.md).

The other way round, an effect line's `{if=...}` condition may name any placeholder of another plugin, such as
`%player_level%`, and it is resolved through PlaceholderAPI.

Without PlaceholderAPI, the plugin runs fully and `/uxmplayerhider doctor` warns that the placeholders publish
nothing.

Related: [Placeholders](../placeholders/reference.md)
