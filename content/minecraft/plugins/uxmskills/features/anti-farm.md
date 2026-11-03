---
title: Anti-farm
order: 407
description: Placed block marks, replacement, bone meal, generators, the heat map, pressure, stillness and the terms that pull up.
icon: shield-check
---

`anti-farm.conf`. Two mechanisms ship and both are on: exact marks close the place and break loop, and decaying terms
answer the player who works the same seam for six hours. Two terms pull the other way. `enabled = false` pins every
term at 1.0 and remembers nothing. `ceiling` (2.0) is the most the whole stack may pay.

| Term | Defaults | Does |
|---|---|---|
| `ledger` | `factor` 0.0, `marks-per-chunk` 4096 | Marks every block a player put down in the chunk's own data, so it survives a restart; breaking it pays `factor` |
| `replace-window` | `factor` 0.0, 5 minutes | Stamps a spot where a natural block was broken, so one put back pays nothing |
| `bone-meal` | `factor` 0.2, 10 minutes | A forced crop pays a fifth |
| `generated` | `factor` 0.05 | A block a cobblestone generator made |
| `heat-map` | cubes of 16, 32 free repeats, 0.97 per repeat, floor 0.01, 10 minutes, 64 cubes a player | How hard you worked each place lately |
| `pressure` | 0.9995 per event, recovers over 20 minutes, floor 0.05 | How hard you worked one skill and activity lately: pace, not place |
| `stillness` | after 3 minutes, `factor` 0.25 | A player who has not moved earns a quarter; never blocked |
| `adjacency` | 3 neighbours, `factor` 1.1 | Pulls up: a natural block beside what was built pays more |
| `field-cycle` | 2 minutes, grown 1.0, rushed 0.35 | Pulls up: a crop with a real growth cycle pays full, one replanted and harvested at once does not |
| `spawner` | `factor` 0.1 | A spawner's mob, followed through conversions |

`/skills rate` and the rate window explain a player's rate and name the term pulling it down most;
`%uxmskills_rate_reason%` gives that term. `uxmskills.bypass` does not exist: staff test with `anti-farm.enabled = false`.

Related: [anti-farm.conf](../config/anti-farm-conf.md)
