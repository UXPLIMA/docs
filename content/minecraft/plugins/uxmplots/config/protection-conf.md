---
title: protection.conf
order: 502
description: Roads, refusals, forbidden blocks, redstone limits and cross-boundary switches.
icon: shield
---

| Key | Default |
|---|---|
| `build-on-roads` | false |
| `build-on-unclaimed` | false |
| `refusal-cooldown-ms` | 2000 |
| `refusals.road`, `unclaimed`, `not-trusted`, `banned`, `edit` | `[]` |
| `forbidden-blocks` | Command blocks, structure blocks and voids, jigsaw, spawners, barrier, light, bedrock, end portal frame, dragon egg |
| `redstone.enabled` | true |
| `redstone.changes-per-second` | 200 |
| `redstone.pause-seconds` | 30 |
| `redstone.paused` | `[]` |
| `cross-boundary.*` | All true |

What each does is in [Protection](../features/protection.md).

Related: [Protection](../features/protection.md)
