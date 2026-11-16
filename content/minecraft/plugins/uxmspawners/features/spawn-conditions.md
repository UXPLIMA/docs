---
title: Spawn Conditions
order: 135
description: When a spawner makes something, set per spawner type by players near, light, time, weather, height, biomes, the owner, its mobs and its level.
icon: list-checks
---

A spawner makes something only while the conditions in its file's `spawner.spawn-conditions` hold.
The shipped files turn on one, a player within 16 blocks, so out of the box a spawner works whenever
someone is near. The others are there to make farms that need the night, a biome or a rank.

```yaml
spawner:
  spawn-conditions:
    players:
      range: 16
      min: 1
      max: 0
    light:
      min: 0
      max: 15
      type: BOTH
    time: [ANY]
    weather: ANY
    height: {}
    biomes:
      list-type: BLACKLIST
      list: []
    permission:
      node: ""
      display: ""
    owner-online: false
    max-mobs: 0
    min-level: 1
    do-not-check-in-nether: true
```

A condition left as above is off. A value that cannot be used is reported in the console with its
file, and a working value is used instead. Changes apply with `/usp reload`.

## The conditions

| Condition | Holds while |
|---|---|
| `players` | At least `min` and at most `max` players are within `range` blocks of the spawner. `min` is at least 1; `max: 0` is no limit. A WildLoaders chunk loader in the chunk counts as enough players, never as too many |
| `light` | The light of the block above the spawner is from `min` to `max`, 0 to 15. `type`: `BLOCK` for torches and lava, `SKY` for daylight, darker at night and in rain, `BOTH` the higher of the two, as the game shows it |
| `time` | The world's time is in one of the listed parts of the day: `DAWN` (23000-999), `MORNING` (1000-5999), `NOON` (6000-8999), `DAY` (0-12999), `NIGHT` (13000-22999), or a range of ticks such as `"18000-20000"`. `[ANY]` is off |
| `weather` | `CLEAR`, `RAIN` (rain or a thunderstorm) or `THUNDER`. `ANY` is off |
| `height` | The spawner is at a Y from `min` to `max`, such as `{min: -64, max: 0}`. Leave one out for no limit on that side |
| `biomes` | The spawner is in a biome of `list` (`WHITELIST`), or in none of them (`BLACKLIST`). Names like `plains` or `minecraft:plains` |
| `permission` | The owner has `node`, also while offline. `display` is how the menu names it, such as `VIP rank` for `group.vip` |
| `owner-online` | The owner is online |
| `max-mobs` | Fewer than this many of the spawner's own mobs are alive. `0` is off. `production.max-living-mobs` caps every spawner too; the lower one counts. With Auto Kill on it never stops the spawner |
| `min-level` | The spawner is at least this level |

The Nether has no day and night and no rain, so with `do-not-check-in-nether: true`, the default,
`time` and `weather` are not checked there.

While a condition does not hold, the spawner checks again every `production.idle-check-interval`
seconds. A Spawn Amount boost never takes it past `max-mobs`.

## What players see

The clock in the middle of the spawner's menu lists the conditions that are on, each with a tick or
a cross and what there is now: the players near, the light level, the time, the biome. Its name says
whether the spawner works or waits. It also names what stops a spawner whatever its conditions say:
its owner stopped it, the world is on Peaceful, its file is missing or turned off, or its ItemsAdder
or Nexo block is not known.

`/usp debug` shows the same list. The texts are `spawn-conditions` in the language file; `names` holds
the words for the parts of the day and the weathers.

## Examples

A spawner that works only at night, in the dark, for VIP owners:

```yaml
    time: [NIGHT]
    light:
      min: 0
      max: 7
      type: SKY
    permission:
      node: "group.vip"
      display: "VIP rank"
```

A farm for at most two players at a time, from level 5:

```yaml
    players:
      range: 16
      min: 1
      max: 2
    min-level: 5
```

Related: [Production](production.md), [Spawner Types](../config/spawner-types.md)
