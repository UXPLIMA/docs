---
title: Overview
order: 201
description: The two roots, renaming them, arguments, and what runs from the console.
icon: list-tree
---

uxmKotl has two commands. Players use `/kotl`; operators use `/uxmkotl`.

| Root | Aliases | Page |
|---|---|---|
| `/kotl` | `/ladder` | [Player commands](player.md) |
| `/uxmkotl` | `/kotladmin` | [Operator commands](admin.md) |

## Commands

### Renaming

Every name, alias and subcommand is set in [`commands.conf`](../config/commands-conf.md). The key on the left
never changes; the name on the right is yours:

```hocon
kotl {
  name    = "kotl"
  aliases = ["ladder"]
  enabled = true
  subcommands {
    join { name = "join" }
  }
}
```

A subcommand set to `enabled = false` leaves the tree while the rest of the command stays. A whole root set to
`enabled = false` is never registered. A change to the file needs a restart: the server builds its command tree
once, at start.

### Words per language

`localized-aliases` gives a command a word for the readers of one language:

```hocon
kotl {
  localized-aliases { tr = ["tepe"] }
}
```

The word works for anyone who types it, but only a player who reads that language finds it in their command list.
A word under `subcommands` is offered to, and works for, the readers of that language and the console only. The
language is the one the player chose, or their client's.

### Arguments

| Argument | Accepts |
|---|---|
| `<arena>` | An arena id, written in lower case as it is stored. Suggestions list the arenas |
| `<world>` | A loaded world. Suggestions list them |
| `<piece>` | `corner1`, `corner2`, `plate`, `spawn`, `exit`, `world`, `name`, `join-on-enter`, `clear-inventory`, `clear-effects`, `mode` |
| `<value>` of `set` | The rest of the line: three whole numbers, `here`, a world, a name, a switch or a mode |
| `<board>` | `held`, `longest`, `claims` or `kills` |
| `<period>` | `alltime`, `monthly`, `weekly` or `daily` |
| `<player>` | A player name. Suggestions list online players |
| `<language>` | A language this server has a messages file for |

### Console

Of the player commands, the bare `/kotl`, `/kotl top` and the two language commands for another player run from the
console; `join`, `leave` and `stats` need a player. Of the operator commands, everything runs from the console except
`edit` and `tp`, and a `set` with the value `here`. `create` from the console needs a world named:
`/uxmkotl create hill world`.

## Permissions

Every player command needs `uxmkotl.play`, which every player has by default. The language subcommands need
`uxmkotl.language`, also everyone's, and `uxmkotl.admin.language` for another player. Each operator subcommand has
its own `uxmkotl.admin.<action>` node, all `op` by default. See the [permission reference](../permissions/reference.md).

## Notes

- **The bare roots answer.** `/kotl` alone lists the arenas that are switched on. `/uxmkotl` alone does what
  `/uxmkotl list` does, but it is gated by `uxmkotl.admin.version`, not `uxmkotl.admin.list`.
- **Replies come from the messages files,** in the language of whoever typed the command. See
  [Messages](../config/messages.md).

Related: [Player commands](player.md), [Operator commands](admin.md), [commands.conf](../config/commands-conf.md)
