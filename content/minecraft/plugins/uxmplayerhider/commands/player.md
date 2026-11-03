---
title: Player commands
order: 202
description: Every /hide subcommand, with its arguments, what it does and the permission it needs.
icon: user
---

`/hide`, aliases `/playerhider` and `/ph`. Every line needs `uxmplayerhider.use`, which everyone has.

| Command | Also needs | What it does |
|---|---|---|
| `/hide` | | Moves to the next state: `all`, `important`, `none`, then `all` again. Player only |
| `/hide state <all\|important\|none>` | | Moves to that state. Player only |
| `/hide status` | | Says which state applies here and how many players it hides right now. Player only |
| `/hide lang` | `uxmplayerhider.language` | Says which language you read uxmPlayerHider in, and which the server has. Player only |
| `/hide lang set <language>` | `uxmplayerhider.language` | Reads uxmPlayerHider in that language from now on. Player only |
| `/hide lang reset` | `uxmplayerhider.language` | Forgets your choice; you read your client's language again. Player only |
| `/hide lang set-player <player> <language>` | `uxmplayerhider.admin.language` | Sets another player's language |
| `/hide lang reset-player <player>` | `uxmplayerhider.admin.language` | Clears another player's choice |

## state

`state` takes `all`, `important` or `none`, and tab completion offers them. Another word answers
`command.not-one-of` with the three. Choosing the state you are in is still a change: it costs the cooldown, runs the
state's effects and applies the whole state again, which repairs a view another plugin disturbed.

A command block or another plugin can set a state with `execute as <player> run hide state none`.

## status

```
/hide status
You are showing only the people who matter. Hidden right now: 12.
```

The state is the one that applies where the player stands: outside the [worlds](../hiding/worlds-and-cooldown.md#worlds)
it is always everybody. The count is of the players online now.

## When a change is refused

| Reply | Key | Why |
|---|---|---|
| `Wait 2 more seconds.` | `error.cooldown` | The last change was less than `cooldown.ms` ago. The seconds are rounded up |
| `You cannot hide players in this world.` | `error.world-not-allowed` | The world is not in `worlds` |

Related: [Overview](overview.md), [States](../hiding/states.md), [Languages](../config/languages.md)
