---
title: Placement rules
order: 207
description: Which worlds builds may go in, between which heights, and how far from spawn.
icon: map-pin
---

Placement rules decide where builds may go at all, before land protection is asked. They drive the
preview colour and the final check at the start, so a green preview never turns into a refusal.

## The rules

| Key | Default | A build is refused when |
|---|---|---|
| `restrictions.worlds.mode` | `NONE` | `WHITELIST`: its world is not listed. `BLACKLIST`: its world is listed |
| `restrictions.worlds.list` | `[ ]` | The world names the mode refers to, case-insensitive |
| `restrictions.minY` | `-64` | Any part of it goes below this height |
| `restrictions.maxY` | `320` | Any part of it goes above this height |
| `restrictions.spawnDistance` | `0` | Its nearest block is closer to the world spawn than this; `0` is off |

The spawn distance is measured horizontally to the nearest edge of the footprint, not to its centre, so
a large castle cannot hang its corner over spawn.

## Examples

Builds only in the survival world, away from the spawn town:

```yaml
restrictions:
  worlds:
    mode: WHITELIST
    list: [ world ]
  spawnDistance: 150
```

Keep builds out of the nether and the end, and under the cloud layer:

```yaml
restrictions:
  worlds:
    mode: BLACKLIST
    list: [ world_nether, world_the_end ]
  maxY: 190
```

## What the player sees

The preview turns red and the chat says which rule refused it, once, with the numbers:

| Message | Placeholders |
|---|---|
| `errorWorldNotAllowed` | `%location.world%` |
| `errorHeightLimit` | `%height.min%`, `%height.max%` |
| `errorTooCloseToSpawn` | `%spawn.distance%`, `%spawn.current%` |

## Notes

- `uxmbuilders.bypass.restrictions`, default operators, ignores every rule. The admin `build` command
  ignores them too.
- The rules apply when a build starts. Changing them never cancels a build already running.

Related: [Land protection](protection.md), [Preview](preview.md)
