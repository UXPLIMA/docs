---
title: "Envoys"
order: 30
description: "An envoy drops ordinary crates somewhere and takes them away again after a while."
---

An envoy drops ordinary crates somewhere and takes them away again after a while. `envoys/<id>.conf` names the
crate, so everything that crate carries still holds: its key, cooldown, permission, animation and rewards.

| Key | Meaning |
|---|---|
| `name` | The name per language. |
| `crate` | The crate id every dropped block is. |
| `drops` | How many come down. |
| `lifetime-seconds` | How long an uncollected crate stays. |
| `area` | A circle (`world`, `centre-x`, `centre-z`, `radius`) with crates spread evenly across it, or `places = ["world,x,y,z", ...]`, which wins over the circle. |
| `schedule` | `times` of day such as `"13:00"`, or `every-seconds`. Times win. With neither, the envoy runs only when started. |
| `on-start`, `on-collect`, `on-end` | Effect lists, in the grammar `pushback.actions` describes. `@key` names a message key. |

The block that was in a spot before a crate landed is remembered and put back when the crate is collected, runs
out of time, or the server stops.

`/uxmcrates envoy start <envoy>`, `envoy stop <envoy>` and `envoy list` run one by hand, stop one and put every
block back, and show which are running and when the next one is due. They share the permission
`uxmcrates.admin.envoy`.
