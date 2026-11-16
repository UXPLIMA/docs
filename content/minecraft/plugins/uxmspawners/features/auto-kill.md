---
title: Auto Kill
order: 134
description: Turning a spawner into a farm nobody has to stand at, where its loot comes from, the modern and legacy ways it works, and the mobs that do spawn.
icon: swords
---

With Auto Kill on, a spawner kills what it spawns and puts the drops into its storage, so the farm
runs while the owner is away from it, as long as its spawn conditions hold. It only applies to
`ENTITY` spawners.

## Turning it on

Auto Kill is switched per spawner, with the **Auto Kill** button in its menu. `general-settings.yml`:

```yaml
auto-kill:
  enabled: true
  default: false
  force: false
  loot-table-only-auto-kill: true
  permission: uxmspawners.autokill
```

| Setting | Meaning |
|---|---|
| `enabled` | `false` turns Auto Kill off on every spawner and hides the button. What each spawner had chosen is kept for when it comes back |
| `default` | Whether a newly placed spawner starts with it on |
| `force` | Auto Kill is on for every spawner, placed before or after, and nobody can turn it off |
| `permission` | What the owner, or a friend, needs to press the button. Empty lets everyone |
| `loot-table-only-auto-kill` | Loot tables apply only to mobs Auto Kill kills. A mob a player kills drops its normal loot. See [Loot Tables](../config/loot-tables.md) |

However it is turned on, by a player, by `force` or by `enabled`, the spawner's mobs still alive are
killed too, and each pays its loot into the storage. So are its mobs that come back later with their
chunk.

## Where the loot comes from

| The mob has | Its loot |
|---|---|
| A [loot table](../config/loot-tables.md) file | That file |
| None | The game's own loot table for that mob, as Minecraft 1.21.11 and newer roll it |

The game's loot is rolled as a player kill without Looting, whoever is online and whatever they hold:
loot that needs a player kill drops, and nothing raises it. The mob is never on fire, a baby, riding
or sheared, a sheep has one of the natural wool colours, and a slime or magma cube is the size it
spawns at. XP is what the mob gives in the game; slimes and magma cubes give their size. The small
cubes a killed slime splits into are paid too, and a wither gives its nether star.

Drops come out raw: nothing is cooked. Every amount is then multiplied by the spawner's level.

## Modern and legacy

```yaml
use-modern-auto-kill:
  enabled: true
```

in `config.yml`.

| | Modern | Legacy |
|---|---|---|
| A mob appears | No | Yes, and dies on the same tick |
| Loot and XP | Rolled as above | The same roll, after the mob died. The mob itself drops nothing |
| Cost to the server | The roll | A mob spawned and killed every cycle, about twenty times as much |
| Kill rewards in other plugins | None | None: the mob dies without a killer, so no player gets statistics, jobs or quest rewards |

Leave modern on. Legacy only differs in what can be seen.

Mobs from ItemsAdder have no loot table to roll, so they are spawned and killed even with modern on,
as a kill of an online player, and pay what they drop.

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

Mobs never spawn with a rider or a mount. Only a mob's own loot is multiplied by the level: what it
carries, the gear it spawned with or a saddle a player put on it, drops once. A spawner keeps at most
`production.max-living-mobs` of its mobs alive at a time. See [Production](production.md#settings).

A spawner mob killed by a player pays into the spawner's storage, multiplied by its level, like an
auto-killed one. Food comes out cooked when the mob died burning or the player used Fire Aspect.

Related: [Basic Concepts](../getting-started/concepts.md), [Loot Tables](../config/loot-tables.md)
