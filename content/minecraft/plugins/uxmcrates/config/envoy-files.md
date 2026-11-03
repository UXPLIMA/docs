---
title: Envoy files
order: 513
description: Every key of envoys/<id>.conf.
icon: package-open
---

One file per envoy under `envoys/`; `supply-drop.conf` ships.

| Key | Meaning |
|---|---|
| `name` | The name, in the three shapes |
| `crate` | The crate id every dropped block is |
| `drops` | How many come down |
| `lifetime-seconds` | How long an uncollected crate stays |
| `area.world`, `centre-x`, `centre-z`, `radius` | A circle the crates spread across |
| `area.places` | `["world,x,y,z", ...]`: fixed spots. Wins over the circle |
| `schedule.times` | Times of day such as `"13:00"` |
| `schedule.every-seconds` | An interval. Times win. With neither it runs only by hand |
| `on-start`, `on-collect`, `on-end` | Effect lists. `@key` names a messages line |

The shipped envoy drops 8 common crates within 200 blocks of 0, 0 in `world` at 13:00 and 19:00, for ten minutes.

Related: [Envoys](../features/envoys.md)
