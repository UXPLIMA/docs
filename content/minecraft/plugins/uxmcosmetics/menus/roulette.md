---
title: Roulette
order: 605
description: The strip that spins while a box opens, and its settings.
icon: dices
---

A box tile or a station of one box. Three rows: a strip of nine icons in the middle row, grey panes above and below.
The title is `menu.roulette.title`, `Opening a box`.

The box is paid for and the prize written before the first frame. The strip then turns `menu.roulette.spins` (26)
steps, slowing from `start-interval-ms` (60) to `end-interval-ms` (400), plays `tick-sound` each step and `win-sound`
when it lands on the prize. Closing the window keeps the prize.

| File | Holds |
|---|---|
| `menus/roulette.conf` | The title, the rows, the strip's slots (`9-17`) and the filler |
| `menu.roulette` in [config.conf](../config/config-conf.md#menu) | The spin: steps, speed and sounds |

The strip runs on the shared animation clock, so each step is rounded up to whole animation ticks.

Related: [Mystery boxes](../features/mystery-boxes.md)
