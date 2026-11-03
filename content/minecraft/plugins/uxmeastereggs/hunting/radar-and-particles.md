---
title: Radar and particles
order: 404
description: The radar hint, the idle particle on unfound eggs, and what each costs the server.
icon: radar
---

Two helps lead a player to an egg they have not found: the radar, which says how far the nearest one is, and the
idle particle, drawn on the egg itself. Both leave out every egg the player has found, a repeatable one included.

## The radar

```hocon
radar {
  default-on = false
  radius = 48
  interval-ms = 3000
  action-bar = true
}
```

| Key | Shipped | What it does |
|---|---|---|
| `default-on` | `false` | Whether a player who has not used `/eggs radar` has the radar on. Read once at start: a change needs a restart |
| `radius` | `48` | How far it looks, in blocks, in the player's own world |
| `interval-ms` | `3000` | How often the radar task wakes, for every player who has it on |
| `action-bar` | `true` | Shows the hint over the hotbar; `false` sends it as a chat line |

While the radar is on and an unfound egg is within the radius, the player reads *An egg is about 12 blocks away*
every interval. With no egg in reach it says nothing. `/eggs radar` flips the radar for one player and runs
`radar-on` or `radar-off`; the flip lasts until they leave the server.

One task serves every player, so the cost is the number of players with the radar on, not the size of the map.

## The idle particle

```hocon
particles {
  interval-ms = 500
  radius = 24
  max-eggs-per-player = 8
}
```

| Key | Shipped | What it does |
|---|---|---|
| `interval-ms` | `500` | How often the particle task wakes. Raise it first on a busy server: half the rate is half the cost |
| `radius` | `24` | How far from a player an egg is drawn, in blocks |
| `max-eggs-per-player` | `8` | The most eggs drawn for one player at a time, at least 1 |

Each egg is drawn with the `particle` block of its type, only for the players who have not found it. The particle
sits `height` blocks above the middle of the block. A type with no particle name, or a count of `0`, draws nothing.
See [Type files](../config/type-files.md#particle).

The cost is the number of online players times the eggs near each, capped by `max-eggs-per-player`; it does not grow
with the map.

## Reloading

`/uxmeastereggs reload` applies a new `radius`, `action-bar` and `max-eggs-per-player` at once, and restarts a task
only when its interval changed.

Related: [Eggs and finds](overview.md), [Moments](moments.md), [config.conf](../config/config-conf.md#radar)
