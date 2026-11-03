---
title: Overview
order: 201
description: The two roots, renaming them, arguments, and what runs from the console.
icon: list-tree
---

uxmParkour has two commands. Players use `/parkour`; operators use `/uxmparkour`.

| Root | Aliases | Page |
|---|---|---|
| `/parkour` | `/pk`, `/jump` | [Player commands](player.md) |
| `/uxmparkour` | `/uxmpk`, `/parkouradmin` | [Operator commands](admin.md) |

The operator root answers to `<player root>admin` as well, `/parkouradmin`, so an operator who knows the player
command can always reach the operator one.

## Commands

### Renaming

Every name, alias and subcommand is set in [`commands.conf`](../config/commands-conf.md). The key on the left
never changes; the name on the right is yours:

```hocon
parkour {
  name    = "parkour"
  aliases = ["pk", "jump"]
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
parkour {
  localized-aliases { tr = ["parkur"], ru = ["паркур"] }
}
```

The word works for anyone who types it, but only a player who reads that language finds it in their command list.
A word under `subcommands` is offered to, and works for, the readers of that language and the console only. The
language is the one the player chose, or their client's.

### Arguments

| Argument | Accepts |
|---|---|
| `<arena>`, `<name>` | An arena id, in any case. Suggestions list the arenas. |
| `<theme>`, `<difficulty>` | An id from [`config.conf`](../config/config-conf.md#themes), in any case. Suggestions list them. |
| `<board>` | `best`, `jumps` or `runs` |
| `<period>` | `alltime`, `monthly`, `weekly` or `daily` |
| `<player>` | A player name. Suggestions list online players. |
| `<value>` of `set` | The rest of the line: three numbers, a word, or `here` |

### Console

`/parkour top`, `/parkour lang set-player` and `/parkour lang reset-player` run from the console. Of the operator
commands, everything runs from the console except `edit` and `tp`, which open a menu or move a player, and a `set`
with the value `here`. `create` from the console needs a world named: `/uxmparkour create sky world`.

## Permissions

Every player command needs `uxmparkour.play`, which every player has by default. The language subcommands need
`uxmparkour.language`, also everyone's, and `uxmparkour.admin.language` for another player. Each operator
subcommand has its own `uxmparkour.admin.<action>` node, all `op` by default. See the
[permission reference](../permissions/reference.md).

## Notes

- **The bare roots answer.** `/parkour` alone joins a run, like `/parkour join`. `/uxmparkour` alone lists the
  arenas, like `/uxmparkour list`.
- **Replies come from the messages files,** in the language of whoever typed the command. See
  [Messages](../config/messages.md).

Related: [Player commands](player.md), [Operator commands](admin.md), [commands.conf](../config/commands-conf.md)
