---
title: Modules
order: 56
description: The files in modules/, what each holds, and the permission lists four of them share.
icon: blocks
---

Each feature has a file of its own under `modules/`.

| File | What it holds | Documented in |
|---|---|---|
| `settings.yml` | Virtual storage and XP storage defaults | [Storage & Selling](../features/storage.md) |
| `storage_limits.yml` | Item and XP storage limits, with permission tiers | [Storage & Selling](../features/storage.md#limits) |
| `multipliers.yml` | Sell multipliers by permission | [Storage & Selling](../features/storage.md#selling) |
| `autokill.yml` | Auto-kill, its permission, split slimes | [Auto-Kill](../features/auto-kill.md) |
| `boosts.yml` | Boost items | [Boosts](../features/boosts.md) |
| `friends.yml` | Shared access to a spawner | [Friends](../features/friends.md) |
| `hologram.yml` | The text above a spawner | [Holograms](../features/holograms.md) |
| `other.yml` | Breaking, drop chances, the pickaxe, vanilla spawners, auto-pickup, the level limit, titles | [Breaking Rules](../features/breaking.md), [Levels](../features/levels.md) |

Not every `enabled` switch turns its feature off in 4.2.0:

| Switch | Effect |
|---|---|
| `hologram.enabled`, `multipliers.enabled`, `storage-limit.enabled` | Work as expected |
| `auto-kill.enabled` | Only decides, with `force`, whether new spawners start with auto-kill on |
| `friends.enabled`, `boosts.enabled` | Not read. Hide the friends button with `gui: false` in the language file; remove boost entries to stop boosts |

## The permission lists

Four settings share one shape, a default and a map of permission to value:

```yaml
permissions:
  uxmspawners-storage1: 5000
  uxmspawners-storage2: 10000
  uxmspawners-storage3: 20000
```

A player holding several of the nodes gets the highest value, not the sum, whatever the order. A
player holding none gets the default. They are used for item storage limits, XP storage limits, sell
multipliers and drop chances.

The comments in the shipped files ask for ascending order. It does no harm, and keeps the file easy to
read, but the plugin does not need it.

Related: [Permissions](../permissions.md), [config.yml](config-yml.md)
