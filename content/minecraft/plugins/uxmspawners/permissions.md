---
title: Permissions
order: 70
description: Staff nodes, the player nodes and their defaults, the rank lists and a working default group.
icon: key
---

Every node below is declared by the plugin with the default shown. The player nodes are the shipped
names: each one is set in `general-settings.yml`, so you can rename it.

## Staff

| Node | Default | Grants |
|---|---|---|
| `uxmspawners.admin` | op | Every node in this table down to `uxmspawners.seedebugs`. Also breaking vanilla spawners while `disable-vanilla-breaking` is on |
| `uxmspawners.command.give` | op | `/usp give` |
| `uxmspawners.command.giveboost` | op | `/usp giveboost` |
| `uxmspawners.command.givepickaxe` | op | `/usp givepickaxe` |
| `uxmspawners.command.reload` | op | `/usp reload` |
| `uxmspawners.command.debug` | op | `/usp debug` |
| `uxmspawners.command.convert` | op | `/usp convert` |
| `uxmspawners.bypass` | op | `/usp bypass`: using every spawner as its owner |
| `uxmspawners.seedebugs` | op | Seeing the debug messages turned on in `config.yml` |
| `uxmspawners.breakallspawners` | no one | Breaking other players' spawners. `break-spawners.permission` |
| `uxmspawners.bypassbreak` | no one | Breaking without the item `break-management` asks for, and breaking in disabled worlds. `break-management.exempt` |
| `uxmspawners.bypassplace` | no one | Placing in disabled worlds and where a claim plugin would refuse it |

The last three are not given to ops on their own, so an op testing the server breaks and places
like a player. Grant them on purpose.

## Players

| Node | Default | Grants | Set in `general-settings.yml` |
|---|---|---|---|
| `uxmspawners.autokill` | op | Turning Auto Kill on and off | `auto-kill.permission` |
| `uxmspawners.settings.togglestorage` | op | Turning Virtual Storage on and off | `virtual-storage.permission` |
| `uxmspawners.settings.togglexp` | op | Turning XP Storage on and off | `xp-storage.permission` |
| `uxmspawners.friends` | op | Adding friends to a spawner | `friends.permission` |
| `uxmspawners.spawnegg` | op | Changing a natural spawner's mob with a spawn egg | `vanilla-spawner-support.spawn-eggs.permission` |
| `uxmspawners.autopickup` | op | A broken spawner goes into the inventory, while `auto-pickup.enabled` is on | `auto-pickup.permission` |

Leave one of these settings empty and everyone may use that feature. An empty `exempt` or
`break-spawners.permission` works the other way: nobody gets past it.

Placing, opening and using your own spawner, levelling it up, selling, taking items and XP,
stopping it and turning its hologram on or off need no permission.

## Ranks

Four lists give more to players holding a node. The names are examples; write your own, such as
`rank.vip.spawnerstorage`.

| Gives | Example nodes | Set in `general-settings.yml` |
|---|---|---|
| A bigger item storage | `uxmspawners-storage1` to `3` | `storage-limit.permissions` |
| A bigger XP storage | `uxmspawners-xpstorage1` to `3` | `storage-limit.xp-permissions` |
| A sell multiplier | `uxmspawners-1-25x`, `uxmspawners-1-50x` | `multipliers.permissions` |
| A chance to keep a broken spawner | `uxmspawners-drop1`, `uxmspawners-drop2` | `spawner-drop-chance.permissions` and `vanilla-permissions` |

A player holding several nodes of one list gets the highest value, not the sum, so the order in the
file does not matter. A value below the list's default is never used.

These nodes count only when they are given to the player, so being op does not give the best rank.
A `*` node in LuckPerms does count as giving them.

## A default group

```
uxmspawners.autokill
uxmspawners.settings.togglestorage
uxmspawners.settings.togglexp
uxmspawners.friends
```

Without `uxmspawners.autokill`, players can never turn Auto Kill on, and their spawners spawn real
mobs. Leave the storage toggles out if every spawner should keep its storage.

Related: [Commands](commands.md), [Breaking Rules](features/breaking.md)
