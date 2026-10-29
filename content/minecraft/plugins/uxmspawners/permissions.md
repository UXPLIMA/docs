---
title: Permissions
order: 70
description: Staff nodes, the player nodes from the module files, the tier lists and a working default group.
icon: key
---

Most permission names are set in the module files, so the nodes below are the shipped defaults and
you can rename them. None are declared by the plugin: grant them with your permission plugin.

## Staff

| Node | Grants | Set in |
|---|---|---|
| `uxmspawners.admin` | `/uxmspawners` and every subcommand. Also breaking vanilla spawners while `disable-vanilla-breaking` is on | Fixed |
| `uxmspawners.bypassplace` | Placing spawners in `disabled-worlds` and where a protection plugin would refuse it | Fixed |
| `uxmspawners.breakallspawners` | Breaking a spawner that belongs to someone else | `break-spawners.permission` in `modules/other.yml` |
| `uxmspawners.bypassbreak` | Breaking without the pickaxe or silk touch that `break-management` asks for | `break-management.exempt` in `modules/other.yml` |

## Players

| Node | Grants | Set in |
|---|---|---|
| `uxmspawners.autokill` | Turning auto-kill on and off from the menu | `modules/autokill.yml` |
| `uxmspawners.settings.togglestorage` | Turning virtual storage on and off | `modules/settings.yml` |
| `uxmspawners.settings.togglexp` | Turning XP storage on and off | `modules/settings.yml` |
| `uxmspawners.autopickup` | A broken spawner goes into the inventory, while `auto-pickup` is on | `modules/other.yml` |

Placing, opening and using your own spawner, levelling it up, selling and managing friends need no
permission. `friends.permission` in `modules/friends.yml` is not checked in 4.2.0: every owner can add
friends.

## Tiers

Four lists give more to players holding a node. The names are examples; write your own.

| Gives | Example nodes | Set in |
|---|---|---|
| A bigger item storage | `uxmspawners-storage1` to `3` | `modules/storage_limits.yml` |
| A bigger XP storage | `uxmspawners-xpstorage1` to `3` | `modules/storage_limits.yml` |
| A sell multiplier | `uxmspawners-1-25x`, `uxmspawners-1-50x` | `modules/multipliers.yml` |
| A chance to keep a broken spawner | `uxmspawners-drop1`, `uxmspawners-drop2` | `modules/other.yml` |

A player holding several nodes of one list gets the highest value, not the sum, so the order in the
file does not matter. Staff with `*` in their permission plugin hold every node and get the highest
of each list.

Names like `rank.vip.spawnerstorage` work as well as the shipped ones, and make the permission plugin
easier to read.

## A default group

```
uxmspawners.autokill
uxmspawners.settings.togglestorage
uxmspawners.settings.togglexp
```

Without `uxmspawners.autokill`, players can never turn auto-kill on, and their spawners spawn real
mobs. Leave the storage toggles out if every spawner should keep its storage.

Related: [Commands](commands.md), [Breaking Rules](features/breaking.md)
