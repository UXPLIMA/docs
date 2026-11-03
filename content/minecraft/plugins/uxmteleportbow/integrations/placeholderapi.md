---
title: PlaceholderAPI
order: 703
description: The uxmteleportbow expansion, and placeholders from other plugins inside this one.
icon: braces
---

With PlaceholderAPI installed, uxmTeleportBow registers the `uxmteleportbow` expansion at start, and takes it down
again when the plugin stops. Nothing needs downloading with `/papi ecloud`; the expansion is inside the plugin.

| You want | Use |
|---|---|
| A cooldown on an action bar | `%uxmteleportbow_cooldown_seconds%`, `%uxmteleportbow_on_cooldown%` |
| A line only while the bow is in hand | `%uxmteleportbow_holding%` |

Every key is on the [reference](../placeholders/reference.md).

The other way round, an effect line's `{if=...}` condition may name any placeholder of another plugin, such as
`%player_level%`, and it is resolved through PlaceholderAPI.

Without PlaceholderAPI, the plugin runs fully and `/uxmteleportbow doctor` warns that the placeholders publish
nothing.

Related: [Placeholders](../placeholders/reference.md)
