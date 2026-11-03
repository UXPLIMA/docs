---
title: Stats and resources
order: 305
description: The six stats and their traits, stat modifiers, and mana and stamina.
icon: chart-column
---

A stat's value is its points (`value = "level"`), paid by skills' `stat-points`; its traits turn each point into an
effect.

| Stat | Traits per point |
|---|---|
| `strength` | `melee-damage` +0.01, `critical-chance` +0.002 |
| `toughness` | `damage-taken` -0.004, `extra-health` +0.1, `block-strength` +0.006 |
| `dexterity` | `ranged-damage` +0.01, `tool-saving-chance` +0.002 |
| `wisdom` | `experience-gain` +0.004, `enchanting-cost` -0.003, `anvil-cost` -0.003, `potion-duration` +0.004 |
| `fortune` | `double-drop-chance` +0.002, `fishing-luck` +0.02 |
| `endurance` | `stamina` +0.005, `mining-speed` +0.004, `replant-chance` +0.003 |

A stat file has `enabled`, `display` (`icon`, `slot`, `colour`, name and description), `value` and `traits`.
`/skills stats` and the stats window show them.

## Modifiers

`/uxmskills statgive <player> <stat> <name> <amount> <minutes>` adds a named, timed modifier; `stattake` takes it off.
`/uxmskills markitem stat <stat> <amount>` makes the held item carry one.

## Resources

| Resource | Maximum | Regeneration | Every | Starts full | Ships |
|---|---|---|---|---|---|
| `mana` | `50 + total_level * 1.5` | `1 + total_level / 100` | 1000 ms | yes | off |
| `stamina` | `100` | `5` | 500 ms | no | off |

What an ability spends. Both ship off.

Related: [Stat files](../config/content-files.md#stats)
