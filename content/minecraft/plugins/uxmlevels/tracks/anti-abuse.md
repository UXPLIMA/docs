---
title: Anti-abuse
order: 308
description: Placed blocks that pay nothing, idle players, the hourly cap, and the cooldown of an event.
icon: shield-alert
---

## Placed blocks

```hocon
anti-abuse {
  mark-player-placed-blocks = true
  marks-per-chunk = 4096
}
```

| Key | Default | What it does |
|---|---|---|
| `mark-player-placed-blocks` | `true` | A block a player places is marked; breaking it pays no `mining` event |
| `marks-per-chunk` | `4096` | Marks one chunk keeps. When it is full the oldest mark goes |

The mark is kept in the chunk's data and survives a restart. Breaking a marked block clears the mark. Planting counts
as placing: a crop or a sapling a player planted is marked where it was put, so breaking a planted crop pays nothing.
A tree's other logs grew and pay.

## Idle players

`experience.idle-timeout-ms`, 5 minutes by default, is how long a player may be still before `playtime` stops paying
them. Moving to another block or using something counts as not still. `0` turns the check off, and a client left
running earns as much as a player.

## Hourly cap

`experience.hourly-cap`, `0` by default for none, is the most one player may earn on one track in a rolling hour.
Only earned experience counts and is held back; `/uxmlevels give` passes the cap. A player at the cap reads
`levels.hourly-cap-reached`, `You have earned all you can on Mining for now.`, and earns nothing on that track until
the oldest gain of the hour falls out.

## Cooldowns

`cooldown-ms` on an event is the least time between two payments of it to one player. The shipped `chat` pays at most
once a minute and `combat-player` once every two minutes, so a pair of friends cannot farm each other.

Related: [Events](events.md), [config.conf](../config/config-conf.md#anti-abuse)
