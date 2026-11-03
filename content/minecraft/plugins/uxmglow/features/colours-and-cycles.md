---
title: Colours and cycles
order: 301
description: Solid colours, cycles, their limits, intervals and the shared clock.
icon: palette
---

## A solid colour

`/glow set <colour>` or a click on a free square of the picker. The colour needs its node,
`uxmglow.colour.<id>`. A locked square refuses with `error.colour-not-permitted` and a bass note.

## A cycle

`/glow cycle <colours> [interval-ms]` needs `uxmglow.cycle` and every colour named. The outline holds each colour for
the interval and moves to the next, round and round. A cycle is started from the command only; the picker sets solid
colours.

| Limit | Default | Key |
|---|---|---|
| Colours in one cycle | 2 to 8 | `cycle.maximum-colours` |
| Shortest interval | 250 ms | `cycle.minimum-interval-ms` |
| Interval when none is named | 1000 ms | `cycle.default-interval-ms` |

## The clock

One task drives every cycling player, not one task per player. It wakes every `cycle.tick-ms` (250) and paints only
the players whose own interval has come round. A player's interval is rounded up to the next multiple of the tick, and
an interval shorter than the tick is served at the tick rate. The cost of the feature is the number of players who are
cycling; it does not grow with the number of colours they chose.

## Off

`/glow off`, or the barrier in the picker. The choice is cleared; the colours stay the player's for later.

Related: [Player commands](../commands/player.md#cycle), [config.conf](../config/config-conf.md#cycle)
