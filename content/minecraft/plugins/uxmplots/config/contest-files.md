---
title: Contest files
order: 607
description: The keys of contests/<id>.conf, with the shipped example.
icon: trophy
---

`contests/example.conf` ships. Every key is explained in [Contests](../features/contests.md#the-file).

| Block | Keys |
|---|---|
| Words | `name`, `theme`, `description`, `hide-theme-until-building`, `hide-builder-until-result` |
| Entry | `entry-mode`, `fresh-plots` (`world`, `from`, `to`, `afterwards`), `entry-fee` (`currency`, `amount`), `capacity` |
| `schedule` | `announced-at`, `building-hours`, `locked-hours`, `judging-hours`, `repeat` |
| `judging` | `public-share`, `jury-share`, `must-have-stood-for-seconds`, `jury-permission`, `jury`, `categories` |
| `rewards` | Rungs: `from`, `to`, `money`, `pot-share`, `commands`, `effects` |
| `effects` | `announced`, `building`, `locked`, `judging`, `closed` |

The example's rewards: first 32 diamonds, a diamond block and half the pot; second 16 and 0.3; third 8 and 0.2; fourth
to 999th one diamond. `/contest edit <contest>` changes a file in game.

Related: [Contests](../features/contests.md)
