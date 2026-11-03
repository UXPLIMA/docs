---
title: Admin commands
order: 203
description: Every /uxmminions subcommand, with its arguments, node and what it does.
icon: shield
---

Every node here is operators only by default and hides its subcommand from anyone who lacks it.

| Command | Node | What it does |
|---|---|---|
| `/uxmminions` | `uxmminions.admin.version` | The usage line |
| `/uxmminions give <player> <type> [level]` | `uxmminions.admin.give` | Gives a minion item, at level 1 by default; refused when their inventory is full |
| `/uxmminions find <id>` | `uxmminions.admin.find` | Where one minion stands |
| `/uxmminions open <id>` | `uxmminions.admin.open` | Opens its panel from the operator's side |
| `/uxmminions minions <player>` | `uxmminions.admin.minions` | The [admin window](../menus/admin.md) of a player's minions |
| `/uxmminions remove <id>` | `uxmminions.admin.remove` | Takes one minion away, written to the audit |
| `/uxmminions restore <id>` | `uxmminions.admin.restore` | Gives one minion back to its owner as an item |
| `/uxmminions slots <player> <amount>` | `uxmminions.admin.slots` | Grants extra slots on top of the total |
| `/uxmminions ledger <player or id>` | `uxmminions.admin.ledger` | What a player's minions paid them, or what one minion paid its owner |
| `/uxmminions migrate <jetsminions\|litminions> [source]` | `uxmminions.admin.migrate` | Reads another plugin's minions into ours |
| `/uxmminions editor` | `uxmminions.admin.editor` | The [in-game editor](../features/editor.md) |
| `/uxmminions reload` | `uxmminions.admin.reload` | Re-reads every file and says what that changed |
| `/uxmminions doctor` | `uxmminions.admin.doctor` | One line per check |
| `/uxmminions version` | `uxmminions.admin.version` | The version this server runs |

`open`, `minions` and `editor` need a player; the rest run from the console.

## restore

For a minion a player says has vanished: its row is given back to the owner as an item at the level it had. The audit
says who moved it last.

## migrate

See [Migrating](../features/migrating.md).

## reload

Reads the configuration again and says how many types, fuel grades, suits and windows it now has. It then lists what is
read once and still needs a restart:

- the storage backend and the table prefix;
- `commands.conf`;
- every interval: the walk, the sweeps and the claim watch;
- `prices.sources`, and `prices.conf` behind it;
- `claims.disabled-providers`;
- `ledger.enabled`.

`editor.enabled` takes a restart too. A file that is wrong keeps the last good snapshot.

Related: [Player commands](player.md), [Permissions](../permissions/reference.md)
