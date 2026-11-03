---
title: Trail
order: 304
description: The line of particles behind the arrow in flight, how smooth it is, and what it costs.
icon: sparkle
---

```hocon
trail {
  enabled  = true
  particle = "END_ROD"
  count    = 2
  offset   = 0.0
  speed    = 0.0
  tick-ms  = 100
  give-up-after-ms = 30000
}
```

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Draw the trail at all |
| `particle` | `"END_ROD"` | The particle, and for one that needs it, its data after a space: `"dust #00ff00"` for a coloured dust, `"block oak_log"` for a block |
| `count` | `2` | Particles drawn at each point; `0` or more |
| `offset` | `0.0` | How far they spread around the point, in blocks; `0` or more |
| `speed` | `0.0` | The particle's own speed; `0` or more |
| `tick-ms` | `100` | Milliseconds between two points of the line; more than `0`. Read at start only |
| `give-up-after-ms` | `30000` | Stop drawing an arrow the plugin never saw land; more than `0`. Read at start only |

## How the line is drawn

One task draws every arrow in flight, every `tick-ms`, at the point where each arrow is at that moment. The gap
between two points is the arrow's speed times `tick-ms`, so a lower value is a smoother line and more work; at the
shipped 100 an arrow leaves a dotted line, and an arrow that lands within the first tick leaves none. The cost grows
with the number of arrows in the air, not with the number of players.

An arrow that leaves the loaded chunks, or that another plugin removes, never lands. `give-up-after-ms` stops drawing
it after 30 seconds rather than for as long as the server runs.

## Notes

- **A particle name the server does not know** draws nothing, and is not tried again until the next start.
- **`tick-ms` and `give-up-after-ms` need a restart.** The drawing task is made once, at start; a reload reads the
  particle, count, offset, speed and the switch.
- **The trail is not an effect line.** The `departure` and `arrival` particles are written in
  [Effects](effects.md).

Related: [Shooting](shooting.md), [config.conf](../config/config-conf.md#trail)
