---
title: Modules
order: 56
description: The module files, and the switches every module shares.
icon: blocks
---

Each feature is one file under `modules/`, so a feature is turned off by one line in one place.

| File | What it holds | Documented in |
|---|---|---|
| `settings.yml` | Virtual storage and XP storage defaults | [Storage & Selling](../features/storage.md) |
| `storage_limits.yml` | Item and XP storage caps, with permission tiers | [Storage & Selling](../features/storage.md) |
| `multipliers.yml` | Permission-based sell multipliers | [Storage & Selling](../features/storage.md) |
| `autokill.yml` | Auto-kill, split entities, loot table scope | [Auto-Kill](../features/auto-kill.md) |
| `boosts.yml` | Boost item definitions | [Boosts](../features/boosts.md) |
| `friends.yml` | Shared access to a spawner | [Friends](../features/friends.md) |
| `hologram.yml` | The floating text above a spawner | [Holograms](../features/holograms.md) |
| `other.yml` | Breaking rules, drop chances, vanilla support, auto-pickup, titles | [Breaking Rules](../features/breaking.md) |

## The pattern the permission lists follow

Three of these files use the same shape — a default, then a map of permission to value:

```yaml
permissions:
  uxmspawners-storage1: 5000
  uxmspawners-storage2: 10000
  uxmspawners-storage3: 20000
```

Two rules apply everywhere it appears: keep the values in ascending order, and understand that a
player holding several nodes gets the highest, not the sum. It is used for storage limits, XP
limits, sell multipliers and spawner drop chance.
