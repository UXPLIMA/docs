---
title: Particles
order: 306
description: Trails, halos and cloaks, the six patterns, colours, rainbow, and the per-player particle cap.
icon: sparkle
---

Trails, halos and cloaks are particles drawn around the player every `period-ms`.

| Family | Pattern | Drawn |
|---|---|---|
| Trails | `TRAIL` | At the feet while walking, falling behind |
| Halos | `HALO` | A ring over the head |
| Cloaks | `CLOAK` or `WINGS` | A cape or a pair of wings behind the back, turning with the player |

`ORBIT` (one point going round) and `HELIX` (a rising pair of lines) are also accepted.

## Keys

| Key | Meaning |
|---|---|
| `particle.type` | A particle name: `FLAME`, `END_ROD`, `DUST`, `HEART`, `ELECTRIC_SPARK`, ... |
| `particle.color-from`, `color-to` | `#rrggbb` for a coloured particle, or `color-from = "rainbow"` |
| `particle.size`, `count`, `spread`, `speed` | The particle's own numbers |
| `pattern` | The figure |
| `radius` | A halo's width; half a cape's width; how far a wing tip reaches |
| `points` | Points of a ring; rows of a cape (three points each); segments of a feather |
| `height` | Above the feet: a crown sits at about 2.1, a shoulder at about 1.6 |
| `period-ms` | How often a frame is drawn, rounded up to whole animation ticks |

## Rainbow

`color-from = "rainbow"` walks the whole hue circle once every eight seconds, and each point of a shape takes the colour
a little further round, so a ring shows the spectrum at once. A rainbow suit walks the same circle, so a player wearing
both shows one colour.

## Wings and capes

Both beat: wings flap once a turn, a cape's hem swings. `points` and `period-ms` set the beat: five points at 125 ms is
about three quarters of a second.

## The cap

`animation.max-particles-per-frame` (64) is the most particle points one player may cost in one frame. A halo with
three hundred points still draws, thinner. It stops one bad file from taking the server down.

Related: [Category files](../config/category-files.md#particle-families), [Catalogue](../catalogue/trail.md)
