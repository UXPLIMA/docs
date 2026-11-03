---
title: Operator commands
order: 303
description: Every operator subcommand of /uxmteleportbow, with its arguments, what it does and the permission it needs.
icon: shield
---

Every operator command is under `/uxmteleportbow`, aliases `/tpbow` and `/uxmtb`.

| Command | Permission | What it does |
|---|---|---|
| `/uxmteleportbow give [player]` | `uxmteleportbow.admin.give` | Gives the bow and its arrow to the player, or to you when no player is named |
| `/uxmteleportbow reload` | `uxmteleportbow.admin.reload` | Reads `config.conf`, the messages and the theme again. See [Configuration](../config/index.md#reloading) |
| `/uxmteleportbow doctor` | `uxmteleportbow.admin.doctor` | Says how the plugin is, one line per check |
| `/uxmteleportbow version` | `uxmteleportbow.admin.version` | Prints the version this server runs |
| `/uxmteleportbow lang set-player <player> <language>` | `uxmteleportbow.admin.language` | Sets the language another player reads |
| `/uxmteleportbow lang reset-player <player>` | `uxmteleportbow.admin.language` | Clears another player's choice |
| `/uxmteleportbow` | `uxmteleportbow.admin.version` | Points to `/uxmteleportbow help` |

## give

`give` does exactly what joining does, with one difference: it ignores the world filter, so an operator can hand the
bow out in a world the filter closes. The bow goes to its slot when that slot is empty or already holds the bow;
otherwise to the slot the player moved it to, or the first free slot. Another item is never written over, and a player
who has the bow does not get a second one. The arrow follows the same rule when `item.ammo.enabled` is on.

```
/uxmteleportbow give Steve
The teleport bow is now with Steve.
```

From the console a player must be named; without one the reply is `error.player-only`.

## doctor

```
How this plugin is
ok  placeholders: published
Everything answered.
```

| Check | ok | warn |
|---|---|---|
| `placeholders` | PlaceholderAPI is there and the placeholders answer | PlaceholderAPI is not on the server; everything else works |

## Another player's language

`lang set-player` and `lang reset-player` take the name of anyone who has played on the server. A player who is
away gets the change at their next join (`language.set-player-away`, `language.reset-player-away`). A name nobody
has played under answers `language.unknown-player`.

Related: [Overview](overview.md), [Permissions](../permissions/reference.md)
