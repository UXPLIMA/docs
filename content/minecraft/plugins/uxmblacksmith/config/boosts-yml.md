---
title: modules/boosts.yml
order: 304
description: The boost item definitions, and the one thing to keep stable.
icon: zap
---

One file, one map, five shipped entries. The full feature is on [Boosts](../features/boosts.md);
this page is the file itself.

```yaml
boosts:
  apprentice_rush:
    id: apprentice_rush
    material: BLAZE_POWDER
    customModelData: -1
    name: '&6&lAPPRENTICE RUSH &8(&f2x&8, &f15m&8)'
    lore:
      - ''
      - '&8• &f&lPROFILE'
      - '&7A short burst for early queue pushes.'
      - ''
      - '&8• &f&lEFFECT'
      - '&72x faster trade progress for 15 minutes.'
      - ''
      - '&a[Right click to activate]'
    type: TIME
    multiplier: 2
    duration: 15
    uses: 0
```

| Key | Applies to | Notes |
|---|---|---|
| `id` | both | Used by `/blacksmith giveboost` |
| `material` | both | Bukkit material |
| `customModelData` | both | `-1` for none |
| `name` | both | `{uses}` is substituted |
| `lore` | both | `{uses}` is substituted |
| `type` | both | `TIME` or `INSTANT` |
| `multiplier` | `TIME` | Speed multiplier |
| `duration` | `TIME` | **Minutes** |
| `uses` | `INSTANT` | Completions carried |

## Keep the id stable

The id is written into the item's persistent data when it is created. Rename the key and every boost
item already in a player's inventory or an unopened crate stops being recognised: it becomes an
ordinary emerald.

Change the name, the lore, the material and the numbers freely; those are read from config each time
the item is used. Change the id never.

## Adding one

```yaml
  weekend_forge:
    id: weekend_forge
    material: GOLDEN_APPLE
    customModelData: -1
    name: '&e&lWEEKEND FORGE &8(&f5x&8, &f120m&8)'
    lore:
      - ''
      - '&75x faster trade progress for 2 hours.'
      - ''
      - '&a[Right click to activate]'
    type: TIME
    multiplier: 5
    duration: 120
    uses: 0
```

Reload, then `/blacksmith giveboost <player> weekend_forge`. There is no separate registration.

## Balance

A `TIME` boost's real value is `multiplier x duration` weighted by how much of the player's queue is
running when they use it. The shipped five are deliberately ordered so the premium one is best only
if you have work queued:

| Boost | Multiplier x minutes |
|---|---|
| `apprentice_rush` | 30 |
| `forge_momentum` | 90 |
| `master_clockwork` | 240 |

An `INSTANT` boost is worth whatever the longest trade in the game takes, which makes it much
stronger than it looks on a server with 6-hour recipes. Price it against that, not against the
`TIME` boosts.
