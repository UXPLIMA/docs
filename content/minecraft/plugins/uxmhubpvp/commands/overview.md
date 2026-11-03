---
title: Overview
order: 301
description: The two roots, renaming them, arguments, and what runs from the console.
icon: list-tree
---

uxmHubPvP has two commands. Players use `/pvp`; operators use `/uxmhubpvp`.

| Root | Aliases | Page |
|---|---|---|
| `/pvp` | `/fight` | [Player commands](player.md) |
| `/uxmhubpvp` | `/hubpvp`, `/pvpadmin` | [Operator commands](admin.md) |

## Commands

### Renaming

Every name, alias and subcommand is set in [`commands.conf`](../config/commands-conf.md). The key on the left never
changes; the name on the right is yours:

```hocon
pvp {
  name    = "pvp"
  aliases = ["fight"]
  enabled = true
  subcommands {
    stats { name = "stats" }
  }
}
```

A subcommand set to `enabled = false` leaves the tree while the rest of the command stays. A whole root set to
`enabled = false` is never registered. A change to the file needs a restart: the server builds its command tree once,
at start.

A renamed `/pvp` keeps working for a tagged fighter only if its new name is in `combat-tag.allowed-commands`; the list
names labels, not commands. See [Combat tag](../fighting/combat-tag.md).

### Words per language

`localized-aliases` gives a command a word for the readers of one language:

```hocon
pvp {
  localized-aliases { tr = ["savas"] }
}
```

The word works for anyone who types it, but only a player who reads that language finds it in their command list.
A word under `subcommands` is offered to, and works for, the readers of that language and the console only.

### Arguments

| Argument | Accepts |
|---|---|
| `<player>` | A player name. `stats` reads any player the server has seen; `reset` any player who has played here |
| `<board>` | `kills` or `streak` |
| `<period>` | `alltime`, `monthly`, `weekly` or `daily` |
| `<language>` | A language this server has a messages file for |

### Console

`/pvp stats <player>`, `/pvp top` and the two language commands for another player run from the console; the bare
`/pvp`, `/pvp stats` without a name and the own-language commands need a player. Every operator command runs from the
console.

## Permissions

The node on a root guards every branch under it. A node held by everybody (`default: true`) keeps its command visible, and a sender without it is refused with
`command.no-permission`; any other node hides its command from a sender without it.
A hidden command is missing from tab completion and the server answers as if it did not exist. Every `/pvp` command needs
`uxmhubpvp.use`; `stats` and `top` need `uxmhubpvp.stats` and `uxmhubpvp.top` as well. Every operator command has its
own `uxmhubpvp.admin.<action>` node. See the [permission reference](../permissions/reference.md).

## Notes

- **The bare roots answer.** `/pvp` alone presses the toggle. `/uxmhubpvp` alone points to `/uxmhubpvp help`.
- **Every root has a help page.** `help` after a root lists its subcommands.
- **Replies come from the messages files,** in the language of whoever typed the command. See
  [Messages](../config/messages.md).

Related: [Player commands](player.md), [Operator commands](admin.md), [commands.conf](../config/commands-conf.md)
