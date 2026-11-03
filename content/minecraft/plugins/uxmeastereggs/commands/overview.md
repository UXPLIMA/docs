---
title: Overview
order: 301
description: The two roots, renaming them, arguments, egg ids, and what runs from the console.
icon: list-tree
---

uxmEasterEggs has two commands. Players use `/eggs`; operators use `/uxmeastereggs`.

| Root | Aliases | Page |
|---|---|---|
| `/eggs` | `/easteregg`, `/eastereggs` | [Player commands](player.md) |
| `/uxmeastereggs` | `/uxmeggs`, `/eggsadmin` | [Operator commands](admin.md) |

## Commands

### Renaming

Every name, alias and subcommand is set in [`commands.conf`](../config/commands-conf.md). The key on the left never
changes; the name on the right is yours:

```hocon
eggs {
  name    = "eggs"
  aliases = ["easteregg", "eastereggs"]
  enabled = true
  subcommands {
    radar { name = "radar" }
  }
}
```

A subcommand set to `enabled = false` leaves the tree while the rest of the command stays. A whole root set to
`enabled = false` is never registered. A change to the file needs a restart: the server builds its command tree once,
at start.

### Words per language

`localized-aliases` gives a command a word for the readers of one language:

```hocon
eggs {
  localized-aliases { tr = ["yumurta"] }
}
```

The word works for anyone who types it, but only a player who reads that language finds it in their command list. A
word under `subcommands` is offered to, and works for, the readers of that language and the console only.

### Arguments

| Argument | Accepts |
|---|---|
| `<egg>` | An egg id: the whole UUID, its first eight characters as the plugin prints them, or any start of it no other egg shares. Suggestions list the eggs |
| `<type>` | A type name, the file name in `types/` without `.conf`. Suggestions list the types |
| `<block>`, `<value>` of `set position` | `here` or nothing for the block you look at, or `<x> <y> <z> [world]` |
| `<board>` | `found` |
| `<period>` | `alltime`, `monthly`, `weekly` or `daily` |
| `<player>` | A player name |
| `<language>` | A language this server has a messages file for |

"The block you look at" is the first block in your line of sight within six blocks. Without a world, a position is in
the world you stand in; the console must name one.

### Console

`/eggs top`, with or without a board, and the two language commands for another player run from the console; the
bare `/eggs`, `radar` and the own-language commands need a player. Of the operator commands, `editor` and `tp` need a
player, and `create` or `delete` with no block or egg named reads where a player looks.

## Permissions

The node on a root guards every branch under it. A node held by everybody (`default: true`) keeps its command visible, and a sender without it is refused with
`command.no-permission`; any other node hides its command from a sender without it.
Every
`/eggs` command needs `uxmeastereggs.use`, which everyone has. Every `/uxmeastereggs` command needs
`uxmeastereggs.admin` and, for most subcommands, a node of its own as well. See the
[permission reference](../permissions/reference.md).

## Notes

- **The bare roots answer.** `/eggs` alone opens the collection. `/uxmeastereggs` alone does what `list` does.
- **Every root has a help page.** `help` after a root lists the subcommands the sender may use.

Related: [Player commands](player.md), [Operator commands](admin.md), [commands.conf](../config/commands-conf.md)
