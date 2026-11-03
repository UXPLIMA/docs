---
title: Reference
order: 701
description: Every %uxmteleportbow_...% placeholder, what it renders, and what it answers for a player who is away.
icon: braces
---

uxmTeleportBow registers the `uxmteleportbow` expansion when PlaceholderAPI is installed. Every placeholder answers
from memory, so a scoreboard that asks many times a second costs nothing.

| Placeholder | Renders | Player away |
|---|---|---|
| `%uxmteleportbow_holding%` | `true` while the player holds the bow in their main hand, else `false` | `false` |
| `%uxmteleportbow_on_cooldown%` | `true` while the cooldown runs, else `false` | `false` |
| `%uxmteleportbow_cooldown_ms%` | Whole milliseconds left, `0` when the bow is ready | `0` |
| `%uxmteleportbow_cooldown_seconds%` | Seconds left with one decimal, such as `2.4`; `0.0` when ready | `0.0` |

A key the plugin does not have, such as `%uxmteleportbow_cooldown%`, is left in the text as written, which is how a
typo shows itself.

## Examples

A scoreboard or action bar line for the cooldown:

```
Teleport in %uxmteleportbow_cooldown_seconds%s
```

A condition for a plugin that shows a line only while the bow is in hand: `%uxmteleportbow_holding% == true`.

## Notes

- **`holding` follows the hotbar.** It changes when the player picks another hotbar slot, joins, changes world or is
  given the bow. An operator who swaps items in creative mode can leave it wrong until the next slot change.
- **The cooldown is the plugin's own.** After a refund it reads `0` while the sweep on the bow still runs.

Related: [PlaceholderAPI](../integrations/placeholderapi.md)
