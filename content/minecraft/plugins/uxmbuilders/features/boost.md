---
title: Speed boost
order: 210
description: Paying to run one build faster for a while.
icon: zap
---

A boost multiplies one build's speed for a fixed time. Players buy it from the build's menu or with
`/builders boost [id]`; buying again extends it.

## How it works

| Step | Detail |
|---|---|
| Buy | Costs `boost.price` from the buyer, the owner or a [trusted player](trusted-players.md) |
| Run | The build's speed is multiplied by `boost.multiplier` until the time runs out |
| Extend | Buying again adds `boost.durationSeconds`, up to `boost.maxDurationSeconds` left |
| Cap | `performance.blocksPerTick` still applies; a boost never exceeds the server ceiling |

Only a building or queued job can be boosted. A boost on a queued job starts counting immediately, so it
pays to boost a build that is already running.

## Settings

| Key | Default | Meaning |
|---|---|---|
| `boost.enabled` | `true` | Hide the button and refuse the command when off |
| `boost.multiplier` | `2.0` | The speed multiplier |
| `boost.durationSeconds` | `300` | Time one purchase adds |
| `boost.maxDurationSeconds` | `1800` | The most time a build can have left |
| `boost.price` | `500.0` | Price per purchase; free when no economy is installed |

## Notes

- The boost runs on the clock, not on build time: it keeps counting while the build is paused, and
  across a restart.
- The job menu's boost button shows the time left while a boost runs.
- Other plugins can change the price or refuse a boost through `BuildBoostEvent`; see the
  [developer API](../developer.md).
- Every purchase is written to the audit log.

Related: [Building and performance](building.md), [Trusted players](trusted-players.md)
