---
title: Auto Kill
order: 134
description: Turning a spawner into a farm nobody has to stand at, the modern and legacy ways it works, and the mobs that do spawn.
icon: swords
---

With Auto Kill on, a spawner kills what it spawns and puts the drops into its storage, so the farm
runs while the owner is away from it, as long as someone is within its range. It only applies to
`ENTITY` spawners.

## Turning it on

Auto Kill is switched per spawner, with the **Auto Kill** button in its menu. `general-settings.yml`:

```yaml
auto-kill:
  enabled: true
  default: false
  enable-for-split-entities: false
  force: false
  kill-living-mobs: true
  loot-table-only-auto-kill: true
  permission: uxmspawners.autokill
```

| Setting | Meaning |
|---|---|
| `enabled` | `false` turns Auto Kill off on every spawner and hides the button. What each spawner had chosen is kept for when it comes back |
| `default` | Whether a newly placed spawner starts with it on |
| `force` | Auto Kill is on for every spawner, placed before or after, and nobody can turn it off |
| `permission` | What the owner, or a friend, needs to press the button. Empty lets everyone |
| `kill-living-mobs` | Turning Auto Kill on also kills the spawner's mobs still alive, and their loot goes to its storage |
| `enable-for-split-entities` | The small slimes and magma cubes a killed one splits into are killed too |
| `loot-table-only-auto-kill` | Loot tables apply only to mobs Auto Kill kills. A mob a player kills drops its normal loot. See [Loot Tables](../config/loot-tables.md) |

## Modern and legacy

```yaml
use-modern-auto-kill:
  enabled: true
```

in `config.yml`.

| | Modern | Legacy |
|---|---|---|
| A mob appears | No | Yes, and dies on the same tick |
| Drops | The mob's loot is rolled directly | The mob's drops when it dies |
| XP | From your loot table, otherwise 1 to 5 | What the mob gives |
| Cost to the server | The roll | A mob spawned and killed every cycle, about twenty times as much |
| Split slimes | Rolled in, with `enable-for-split-entities` | Killed only with `enable-for-split-entities` |
| Kill rewards in other plugins | None | Each kill counts for an online player who is not holding a Looting weapon |

Leave modern on. With legacy, jobs, quest and statistics plugins reward one online player for the
mobs of every spawner on the server.

Both roll loot as if an online player killed the mob, so loot that needs a player kill drops. A
player holding a Looting weapon is never used, so Looting does not raise anyone's loot. With nobody
online, as on a server kept running by chunk loaders, that loot is not rolled.

With a [loot table](../config/loot-tables.md) for the mob, both use it. Without one, modern rolls the
game's own loot table for that mob and legacy uses what the mob dropped. Drops come out raw:
neither cooks food.

Mobs from ItemsAdder have no loot table to roll, so they are spawned and killed even with modern on.

## Mobs that do spawn

Without Auto Kill, or with legacy Auto Kill, mobs exist in the world. These settings in
`general-settings.yml` keep them cheap and harmless:

```yaml
nerf-spawner-mobs:
  enabled: false
  nerf-every-mob: false
  mobs:
    - ZOMBIE

no-equipment:
  enabled: false

spawned-mobs:
  block-equipping: true
```

| Setting | Meaning |
|---|---|
| `nerf-spawner-mobs` | The mobs lose their AI: they do not move, look around, target or attack. A nerfed creeper does not explode |
| `nerf-every-mob: false` | Only the mobs in `mobs` are nerfed |
| `nerf-every-mob: true` | Every mob is nerfed except those in `mobs` |
| `no-equipment` | Spawned mobs carry no armour or weapons |
| `spawned-mobs.block-equipping` | Players cannot saddle, armour, ride, tame or leash spawner mobs, open their inventory or give them items |

Only a mob's own loot is multiplied by the level. What it carries, the gear it spawned with or a
saddle a player put on it, drops once. A spawner keeps at most `production.max-living-mobs` of its
mobs alive at a time. See [Production](production.md#settings).

A spawner mob killed by a player pays into the spawner's storage, multiplied by its level, like an
auto-killed one. Food comes out cooked when the mob died burning or the player used Fire Aspect.

Related: [Basic Concepts](../getting-started/concepts.md), [Loot Tables](../config/loot-tables.md)
