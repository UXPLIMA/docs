---
title: Combat tag
order: 403
description: What tags a fighter, the commands a tagged fighter may still type, and what quitting while tagged costs.
icon: timer
---

A hit between two fighters tags both of them. The tag keeps a player from escaping a fight with a command or by
logging out.

```hocon
combat-tag {
  duration-ms = 10000
  allowed-commands = ["pvp", "msg", "r", "reply", "helpop"]
}
```

| Key | Shipped | What it does |
|---|---|---|
| `duration-ms` | `10000` | How long a tag lasts after the last hit. `0` switches tagging off entirely |
| `allowed-commands` | `pvp`, `msg`, `r`, `reply`, `helpop` | The only commands a tagged player may type |

## How a tag runs

| Event | Tag |
|---|---|
| A fighter hits another fighter | Both are tagged for `duration-ms`. A player newly tagged runs the `combat-tagged` moment: *You are in a fight for 10 seconds.* |
| Another hit while tagged | The time starts again, silently |
| The time runs out | The tag ends |
| The player dies | Their tag ends; so does their killer's |

Only a hit that lands tags. A hit refused because one side is not fighting, or one left to another plugin's game,
tags nobody.

## Commands while tagged

The list is a whitelist: a command that is not named is refused with *Not while you are fighting. 6 seconds left.*
A plugin installed next month cannot open a way out of a fight unless you add its command here.

| Typed | Matched as |
|---|---|
| `/msg Steve hi` | `msg` |
| `/Essentials:MSG Steve hi` | `msg`: the slash, the plugin prefix, the arguments and the case are dropped |
| `/fight` | `fight`: an alias is its own label. The shipped list has `pvp` and not `fight` |

Players with `uxmhubpvp.bypass.commands`, operators by default, type anything.

## Quitting while tagged

A player who leaves the server while tagged gets a death on their record, and their streak ends. Nobody gets the
kill. Their own items still come back, as on any quit.

## Notes

- **The toggle is not blocked.** A tagged fighter can still press the toggle to start the 8 second way out; the
  warm-up and its move limit are what keep them in reach.
- **The tag lives in memory.** A restart clears every tag.

Related: [Turning PvP on and off](overview.md), [Deaths and kills](deaths-and-kills.md), [config.conf](../config/config-conf.md#combat-tag)
