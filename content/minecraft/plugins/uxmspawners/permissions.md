---
title: Permissions
order: 70
icon: key
---

Almost every permission in uxmSpawners is defined in a module file rather than in `plugin.yml`,
which means you can rename any of them. The nodes below are the shipped defaults.

## Staff

| Node | Grants |
|---|---|
| `uxmspawners.admin` | The whole `/uxmspawners` command |
| `uxmspawners.breakallspawners` | Breaking any spawner regardless of owner |
| `uxmspawners.bypassbreak` | Ignoring the break item or silk touch requirement |

## Player features

| Node | Grants | Defined in |
|---|---|---|
| `uxmspawners.autokill` | Toggling auto-kill on your own spawner | `modules/autokill.yml` |
| `uxmspawners.friends` | Adding friends to your own spawner | `modules/friends.yml` |
| `uxmspawners.settings.togglestorage` | Toggling virtual storage | `modules/settings.yml` |
| `uxmspawners.settings.togglexp` | Toggling XP storage | `modules/settings.yml` |
| `uxmspawners.autopickup` | Broken spawners go to the inventory | `modules/other.yml` |

## Tiered nodes

These are lists you write yourself; the names below are only the shipped examples. Highest match
wins, so keep each list ascending.

| Purpose | Example nodes | Defined in |
|---|---|---|
| Item storage limit | `uxmspawners-storage1` … `3` | `modules/storage_limits.yml` |
| XP storage limit | `uxmspawners-xpstorage1` … `3` | `modules/storage_limits.yml` |
| Sell multiplier | `uxmspawners-1-25x`, `uxmspawners-1-50x` | `modules/multipliers.yml` |
| Spawner drop chance | `uxmspawners-drop1`, `uxmspawners-drop2` | `modules/other.yml` |
| Vanilla drop chance | `uxmspawners-drop1`, `uxmspawners-drop2` | `modules/other.yml` |

<Callout type="tip" title="Rename them to match your ranks">

Because these live in config rather than in `plugin.yml`, `uxmspawners-storage2` can just as well
be `rank.gold.spawnerstorage`. Naming them after your ranks makes the permission plugin readable,
and there is no cost to doing it.

</Callout>
