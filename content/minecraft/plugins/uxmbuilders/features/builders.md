---
title: Builders
order: 203
description: Crews with levels, unlock prices, package affinity and an NPC appearance.
icon: hard-hat
---

A builder is the crew a player hires. It decides how fast a job goes, how much material it wastes, and
what the NPC on the site looks like. Players manage theirs in `/builders crew`.

## The shipped builders

| Id | Looks like | Levels | Speed range (blocks/s) | Unlock |
|---|---|---|---|---|
| `apprentice` | A player in a leather helmet | 3 | 2 to 6 | Free, the default |
| `mason` | A player with stone bricks | 5 | 5 to 18 | 50,000 |
| `robot` | A player with redstone, electric sparks | 5 | 8 to 30 | 150,000 and `uxmbuilders.builder.robot` |
| `wizard` | A player with a blaze rod, builds in `RANDOM` order | 5 | 10 to 40 | 250,000 and `uxmbuilders.builder.wizard` |
| `zombie_crew` | A zombie with planks | 4 | 4 to 16 | 100,000 |

Every number is yours to change in [builders.yml](../config/builders-yml.md).

## Levels

Each level sets three things:

| Stat | Effect |
|---|---|
| `blocksPerSecond` | Placement speed, and the NPC's walking speed |
| `efficiency` | The chance a placed block takes no material from the warehouse |
| `price` | What buying this level costs |

`blocksPerSecond` may not fall from one level to the next; the file is refused at load if it does.
However fast a builder is, `performance.blocksPerTick` is the ceiling.

## Owning and upgrading

| Action | How |
|---|---|
| Buy a builder | `/builders crew buy <builder>` or the crew menu; pays `unlock.price` and checks `unlock.permission` |
| Select it | `/builders crew select <builder>`; only owned builders, and only with `builders.allowSwitching` |
| Upgrade it | `/builders crew upgrade <builder>`; pays the next level's `price` |
| Grant it | `uxmbuilders.builder.<id>` or `uxmbuilders.builder.<id>.level.<n>` count as owned |
| Set it as staff | `/builders admin setbuilder <player> <builder> [level]` |

New players start with `builders.default`. A running build keeps the builder and level it started with;
upgrading speeds up the next build, not the current one.

## Package affinity

A package can favour a builder:

```yaml
packages:
  castle_medium:
    builderAffinity:
      mason: 1.15
      robot: 1.05
```

Masons build this package 15% faster. Use it to make builder choice matter for specific packages
rather than raising one builder's speed everywhere.

## The NPC

One NPC per job, sent as packets only: no server entity, no AI, no collision, no entity-count cost. It
walks to each block it places, holds that block, swings, and plays the builder's `placeSound`.

| Setting | Default | Meaning |
|---|---|---|
| `npc.enabled` | `true` | Turn NPCs off entirely; builds continue without them |
| `npc.nameFormat` | `%builder.name%` in yellow bold | The nameplate |
| `npc.viewDistance` | `48` | Who receives the NPC packets |
| `npc.teleportThreshold` | `24` | Beyond this the NPC teleports instead of walking |
| `npc.hideFromTablist` | `true` | Keep player-shaped NPCs out of the tab list |

`appearance.type` is `PLAYER` (with an optional skin `texture` and `signature`) or `MOB` with an
`entity`. A paused job's NPC lies down on the site until the job resumes. `/builders admin npc killall` removes every NPC while
the builds keep running.

Related: [Building and performance](building.md), [builders.yml](../config/builders-yml.md)
