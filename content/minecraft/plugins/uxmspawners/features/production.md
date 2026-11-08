---
title: Production
order: 31
description: When a spawner runs, how much one cycle makes, what a level multiplies, stopping a spawner, and keeping many spawners cheap.
icon: factory
---

The block's own vanilla spawning is switched off: a uxmSpawners spawner never spawns on Minecraft's
timer. The plugin looks at every spawner in a loaded chunk five times a second and runs the ones
that are due.

## When a spawner runs

| Condition | Detail |
|---|---|
| Its chunk is loaded | A spawner in an unloaded chunk waits, and is not loaded for it |
| Its `delay` has passed | Seconds between cycles, from the spawner's file. A Spawn Speed boost divides it |
| A player is within its `range` | Counted in every direction, any player in the world. A WildLoaders chunk loader in its chunk counts as one |
| It is not stopped | The owner can stop it from the menu, see below |
| It has room for mobs | Only for spawners that spawn real mobs, see `max-living-mobs` |

Light, free space and the mobs around it make no difference. `delay` and `range` are set per spawner
type, 8 seconds and 16 blocks by default. See [Spawner Types](../config/spawner-types.md).

## One cycle

| Spawner | One cycle makes |
|---|---|
| `ENTITY`, Auto Kill on, modern | No mob. One mob's loot and XP, times the level, straight to storage |
| `ENTITY`, Auto Kill on, legacy | One mob, killed at once. Its drops and XP times the level go to storage |
| `ENTITY`, Auto Kill off | One mob. Whoever kills it, its drops and XP times the level go to storage |
| `ITEM` | The level's number of the item, to storage |

A Spawn Amount boost adds to the one: `+2` makes three. A fraction counts as a chance, so `+1.5`
adds one or two, half the time each. See [Boosts](boosts.md).

## What a level multiplies

`general-settings.yml`:

```yaml
level-scaling:
  mode: LOOT
  max-mobs-per-cycle: 16
  never-multiplied-items:
    - ZOMBIE_HEAD
    - SKELETON_SKULL
    - CREEPER_HEAD
    - PIGLIN_HEAD
    - "MUSIC_DISC_*"
```

| Setting | Meaning |
|---|---|
| `mode: LOOT` | The usual number of mobs, each paying its drops and XP times the level |
| `mode: MOB_COUNT` | Level times as many mobs, each with normal drops. The total is the same as `LOOT` |
| `max-mobs-per-cycle` | With `MOB_COUNT`, the most mobs one cycle spawns. The rest of the level is added to their drops |
| `never-multiplied-items` | Drops that come once, whatever the level: heads from a charged creeper kill, music discs. Item names, or patterns with `*`. XP is still multiplied |

`mode` only matters for spawners that spawn real mobs. Modern Auto Kill and `ITEM` spawners always
multiply their output by the level.

## Stopping a spawner

The owner stops and starts a spawner with the button in the bottom right corner of its menu. A
stopped spawner makes nothing, and says so in its menu. `allow-stopping-spawners: false` in
`general-settings.yml` hides the button and lets every stopped spawner run again.

## Settings

`general-settings.yml`:

```yaml
production:
  idle-check-interval: 2
  max-living-mobs: 32
  start-spread-seconds: 2
```

| Setting | Meaning |
|---|---|
| `idle-check-interval` | Seconds a spawner with nobody in range waits before it looks again. A player who walks up waits at most this long |
| `max-living-mobs` | The most living mobs one spawner keeps around. It waits while it has that many, so mobs do not pile up at an AFK player. Auto Kill's mobs do not count. `0` is no limit |
| `start-spread-seconds` | Spawners that start together, after a restart or when a player walks into a farm, would all produce in the same tick. Each one's start moves by up to this many seconds. `0` turns it off |

`disable-vanilla-spawners-spawning: true` stops natural spawners, the ones uxmSpawners did not place,
from spawning anything.

## Many spawners

| What | Why |
|---|---|
| Keep `use-modern-auto-kill` on | Rolling loot costs a small part of what spawning and killing a mob does. With thousands of spawners it is the largest saving there is |
| Keep `start-spread-seconds` above 0 | Spreads the work of spawners that run on the same timer over that many seconds instead of one tick |
| Raise `idle-check-interval` | Spawners nobody stands near cost less |
| Raise Paper's `tick-rates.mob-spawner` | Minecraft still ticks every spawner block, natural ones included, to look for players. uxmSpawners spawners do not use that tick for anything, so a higher value in `paper-world-defaults.yml` saves that work. Check your natural spawners after changing it |

Related: [Auto Kill](auto-kill.md), [Levels](levels.md), [Spawner Types](../config/spawner-types.md)
