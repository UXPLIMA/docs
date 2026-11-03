---
title: contests.conf
order: 504
description: The contest boss bar and the daily vote reward.
icon: trophy
---

| Key | Default |
|---|---|
| `boss-bar.enabled` | true |
| `boss-bar.show-to` | `in-a-plot-world` (or `everybody`, `entrants`) |
| `boss-bar.phases` | `announced`, `building`, `judging` |
| `boss-bar.colour` | `blue` |
| `boss-bar.style` | `notched-10` |
| `vote-reward.per-day` | 10 |
| `vote-reward.time-zone` | `""` (the server's) |
| `vote-reward.effects` | A thank you, one diamond, a sound |
| `vote-reward.when-capped` | A line saying the day's rewards are used |

Every contest is its own file; see [Contest files](contest-files.md).

Related: [Contests](../features/contests.md#the-boss-bar-and-vote-reward)
