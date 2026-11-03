---
title: Admin commands
order: 203
description: Every /uxmenchants subcommand, with its arguments, node and what it does.
icon: shield
---

Every node here is operators only by default and hides its subcommand from anyone who lacks it.

| Command | Node | What it does |
|---|---|---|
| `/uxmenchants` | `uxmenchants.admin.version` | Says to add `help` |
| `/uxmenchants version` | `uxmenchants.admin.version` | The version this server runs |
| `/uxmenchants doctor` | `uxmenchants.admin.doctor` | One line per check; see [Installation](../getting-started/installation.md#checking-the-install) |
| `/uxmenchants reload` | `uxmenchants.admin.reload` | Reads `config.conf`, the content, the messages, the theme and the three player windows again |
| `/uxmenchants editor` | `uxmenchants.admin.editor` | Opens the [in-game editor](../features/editor.md) |
| `/uxmenchants apply <enchantment> [level]` | `uxmenchants.admin.apply` | Puts an enchantment on the item in your hand |
| `/uxmenchants remove <enchantment>` | `uxmenchants.admin.remove` | Takes one of ours off the item in your hand |
| `/uxmenchants give <player> <enchantment> [level]` | `uxmenchants.admin.give` | Gives a book |
| `/uxmenchants giverandom <player> <group> [minimum-level] [maximum-level]` | `uxmenchants.admin.give` | Gives a random book from a rarity or a type |
| `/uxmenchants export` | `uxmenchants.admin.export` | Prints what the held item carries |
| `/uxmenchants strip <world>` | `uxmenchants.admin.strip` | Takes every enchantment of ours off the players in one world |
| `/uxmenchants import <lore\|registry>` | `uxmenchants.admin.import` | Brings another plugin's enchantments on online players' items across to ours |

## reload

Takes a few milliseconds and says how many. A content file that does not parse answers `admin.content-broken` with
the file, the key and the line, and nothing changes. `storage`, `editor.enabled` and `commands.conf` need a restart.

## apply and remove

`apply` takes the level (default 1) and runs the same rule as the anvil, so the limit, conflicts, requirements and the
enchantment's node are all asked; a refusal says which rule. `remove` answers `enchant.not-on-item` when the item does
not carry it. Both need an item in the main hand.

## give and giverandom

`give` caps the level at the enchantment's `max-level`. `giverandom` takes a rarity id or, when no rarity has that name,
a type id (`/uxmenchants giverandom Steve legendary 1 2`), picks one enchantment of it at random and a level between the
two numbers (both default to 1), held to that enchantment's range. A word that matches nothing answers
`book.none-matched`.

A full inventory drops the book at the player's feet and says so. Books are the only route that does not ask a node or
an `obtaining` switch: a book is given, and the rule runs when it is applied.

## export

```
What this item carries
sharpened = 3
vein-miner = 2
```

Lines in the form a content file or a test can be written from. An item with nothing of ours answers `export.empty`.

## strip

`/uxmenchants strip world_nether`. Every player in that world now has every item of ours stripped, armour and off hand
included, each on that player's own thread. A second later it says how many items were cleaned. A chest, a dropped item
and a player who is offline keep their data; that data is inert once the content is gone, because nothing but the id and
the level was ever written onto an item.

## import

See [Migrating](../features/migrating.md).

Related: [Player commands](player.md), [Permissions](../permissions/reference.md)
