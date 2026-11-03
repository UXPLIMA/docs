---
title: Worlds and cooldown
order: 403
description: The worlds the plugin works in, what happens outside them, and the gap between two changes.
icon: globe
---

## Worlds

```hocon
worlds = ["lobby", "hub"]
```

| Key | Default | What it does |
|---|---|---|
| `worlds` | `[]` | The worlds the tool and the command work in, matched without regard to case. Empty is every world |

Outside the list:

| | |
|---|---|
| The view | The player sees everybody |
| Their choice | Kept, and applied again when they walk back into a listed world |
| The tool | Taken away, given back in a listed world |
| `/hide`, `/hide state` | Refused with `error.world-not-allowed` |
| `/hide status`, `%uxmplayerhider_state%` | Report `all` |

The view is worked out when the player arrives in a world. A list changed by a reload applies to each player at
their next change, join, respawn or world change.

## Cooldown

```hocon
cooldown {
  ms = 2000
}
```

| Key | Default | What it does |
|---|---|---|
| `cooldown.ms` | `2000` | Milliseconds between two changes of one player; `0` turns it off. Not negative |

A change re-applies the player's view against everybody online, so the cooldown keeps a player from making the server
work by holding the button. A change inside it answers `error.cooldown` with the seconds left, rounded up, and
nothing changes. A refused change because of the world does not start the cooldown.

`uxmplayerhider.cooldown.bypass` (`op` by default) never waits. The cooldown is kept in memory and forgotten when the
player leaves.

Related: [States](states.md), [config.conf](../config/config-conf.md)
