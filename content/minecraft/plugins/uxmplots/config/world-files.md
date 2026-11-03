---
title: World files
order: 606
description: The keys of worlds/<world>.conf, with the shipped creative world.
icon: globe
---

`worlds/creative.conf` ships. Every key is explained in [Plot worlds](../features/worlds.md).

| Block | Keys |
|---|---|
| Grid | `create-world`, `plot-size`, `road-size`, `origin`, `bottom-y`, `layers`, `walls`, `road-material`, `biome` |
| Rules | `max-plots-per-player`, `visit-policy`, `guard-edits`, `starter-template` |
| `pre-generation` | `radius-chunks`, `chunks-per-batch`, `tps-floor` |
| `game-mode` | `mode`, `on-entering`, `on-leaving` |
| Flags | `flags`, `locked-flags` |
| `entities` | `default`, `total`, `per-type` |
| `price` | `first`, `step`, `curve`, `currency`, `refund-share` |
| `patterns` | `floor`, `wall`, `ground`, `border`: each entry `materials`, `tile`, `permission` |
| `biomes` | Each entry `biome`, `permission` |

The in-game editor at `/uxmplots world <world>` writes the rules; see [In-game editors](../features/editors.md).

Related: [Plot worlds](../features/worlds.md)
