---
title: Production
order: 131
description: When a spawner runs, how much one cycle makes, what a level multiplies, stopping a spawner, and keeping many spawners cheap.
icon: factory
---

The block's own vanilla spawning is switched off: a uxmSpawners spawner never spawns on Minecraft's
timer. The plugin looks at the spawners in loaded chunks five times a second and runs the ones that
are due.

## When a spawner runs

| Condition | Detail |
|---|---|
| Its chunk is loaded | A spawner in an unloaded chunk waits, and is not loaded for it |
| Its `delay` has passed | Seconds between cycles, from its file, at least 1. A Spawn Speed boost divides it |
| Its spawn conditions hold | A player within 16 blocks by default, or a WildLoaders chunk loader in the chunk. See [Spawn Conditions](spawn-conditions.md) |
| It is not stopped | The owner can stop it from the menu, see below |
| Its file is there and turned on | A spawner whose file is missing or has `enabled: false` waits until it is back |
| The world is not on Peaceful | Unless `world-management.work-in-peaceful` is on, see below |
| It has room for mobs | Only for spawners that spawn real mobs, see `max-living-mobs` |

The vanilla rules do not apply: free space and the mobs around it make no difference, and light only
when a spawn condition asks for it.

## One cycle

| Spawner | One cycle makes |
|---|---|
| `ENTITY`, Auto Kill on, modern | No mob. One mob's loot and XP, times the level, straight to storage |
| `ENTITY`, Auto Kill on, legacy | One mob, killed at once. Its loot and XP times the level go to storage |
| `ENTITY`, Auto Kill off | One mob. Whoever kills it, its drops and XP times the level go to storage |
| `ITEM` | The level's number of the item, to storage |

A Spawn Amount boost adds to the one: `+2` makes three mobs, or three rolls of loot. A fraction counts
as a chance, so `+1.5` adds one or two, half the time each. See [Boosts](boosts.md).

## What a level multiplies

A spawner's level multiplies the loot and XP of every mob it makes; it never spawns more mobs. An
`ITEM` spawner makes the level's number of items.

```yaml
level-scaling:
  never-multiplied-items:
    - ZOMBIE_HEAD
    - SKELETON_SKULL
    - CREEPER_HEAD
    - PIGLIN_HEAD
    - "MUSIC_DISC_*"
```

in `general-settings.yml`: drops that come once, whatever the level. These are what a mob gives for
how it died, a head from a charged creeper or a music disc, which a high level would otherwise turn
into a stack. Item names, or patterns with `*`. XP is still multiplied.

## Stopping a spawner

The owner stops and starts a spawner with the button in the bottom right corner of its menu. A
stopped spawner makes nothing, and its conditions button says so. `allow-stopping-spawners: false` in
`general-settings.yml` hides the button and lets every stopped spawner run again.

## Peaceful worlds

No monster spawns on Peaceful, so with `world-management.work-in-peaceful: false`, the default,
spawners are off in a Peaceful world: they cannot be placed or broken there and make nothing. With
`true`, they can be placed and broken, and mob spawners with Auto Kill on work: their loot is rolled
without spawning a mob, legacy Auto Kill included. The others, `ITEM` spawners too, wait. The
difficulty is checked as the server runs.

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
| `idle-check-interval` | Seconds a spawner whose conditions do not hold waits before it looks again. Once they hold, it waits at most this long |
| `max-living-mobs` | The most living mobs one spawner keeps around. It waits while it has that many, so mobs do not pile up at an AFK player. Auto Kill's mobs do not count. `0` is no limit. A spawner file's `max-mobs` can set a lower one |
| `start-spread-seconds` | Spawners that start together, after a restart or when a player walks into a farm, would all produce in the same tick. Each one's start moves by up to this many seconds. `0` turns it off |

`vanilla-spawner-support.disable-vanilla-spawning: true` stops natural spawners, the ones
uxmSpawners did not place, from spawning anything.

## Many spawners

| What | Why |
|---|---|
| Keep `use-modern-auto-kill` on | Rolling loot costs a small part of what spawning and killing a mob does. With thousands of spawners it is the largest saving there is |
| Keep `start-spread-seconds` above 0 | Spreads the work of spawners that run on the same timer over that many seconds instead of one tick |
| Raise `idle-check-interval` | Spawners nobody stands near cost less |
| Raise Paper's `tick-rates.mob-spawner` | Minecraft still ticks every spawner block, natural ones included, to look for players. uxmSpawners spawners do not use that tick for anything, so a higher value in `paper-world-defaults.yml` saves that work. Check your natural spawners after changing it |

Related: [Spawn Conditions](spawn-conditions.md), [Auto Kill](auto-kill.md), [Spawner Types](../config/spawner-types.md)
