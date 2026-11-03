---
title: Reference
order: 701
description: Every %uxmplayerhider_...% placeholder, what it renders, and what it answers for a player who is away.
icon: braces
---

uxmPlayerHider registers the `uxmplayerhider` expansion when PlaceholderAPI is installed. Every placeholder answers
from memory.

| Placeholder | Renders | Player away |
|---|---|---|
| `%uxmplayerhider_state%` | The state that applies where the player stands: `all`, `important` or `none`. Outside the listed worlds, `all` | `all` |
| `%uxmplayerhider_hiding%` | `true` when that state is not `all`, else `false` | `false` |
| `%uxmplayerhider_hidden%` | How many online players that state hides from the player right now | `0` |

The state is an id, the same in every language. A key the plugin does not have is left in the text as written, which
is how a typo shows itself.

## Examples

A scoreboard line:

```
Hidden: %uxmplayerhider_hidden%
```

A condition for a plugin that shows a line only to players who hide someone: `%uxmplayerhider_hiding% == true`.

Related: [PlaceholderAPI](../integrations/placeholderapi.md), [States](../hiding/states.md)
