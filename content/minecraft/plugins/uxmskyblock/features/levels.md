---
title: Level and leaderboards
order: 405
description: Worth, points and level, the three boards and leaderboard displays.
icon: trophy
---

## Worth and level

Every block an island holds adds worth and points; points make levels along the curve in
[levels.conf](../config/levels-conf.md). `/is level` shows the level, `/is level recalculate` counts the blocks again,
`/is worth` shows the worth and `/is values` opens what each block adds. A stack counts as many blocks as it holds; see
[stacking.conf](../config/stacking-conf.md).

## The boards

| Board | Ranks islands by |
|---|---|
| `level` | Island level |
| `worth` | Island worth |
| `bank` | Money in the bank |

`/is top` opens them. A place is an island, shown under its owner's name and head. The boards are kept for all time,
and seasons keep boards of their own; see [Missions and rewards](missions.md#seasons). How often they are counted is
in `leaderboard` in [config.conf](../config/config-conf.md).

## Displays in the world

`/isadmin top add <board> <rank> [sign | head | text]` makes the sign or head you look at, or the spot you stand on,
show a place of a board. `/isadmin top remove` takes one away and `/isadmin top list` lists them. The lines are
`top_display` in the language files.

The boards are also [placeholders](../placeholders/reference.md#leaderboards).
