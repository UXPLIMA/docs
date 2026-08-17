---
title: Perks
order: 209
description: "The buyable upgrade tree: effect types, costs, and the shipped nine."
icon: git-branch
---

Levelling grants perk points. Points buy perk levels. Perks are defined under `progression.perks` in
`config.yml` and there is no limit on how many you add.

## The nine shipped perks

| Key | Name | Effect | Per level | Max | Unlocks at |
|---|---|---|---|---|---|
| `speed_forging` | Swift Hammer | `TRADE_SPEED` | 5% faster | 5 | Level 1 |
| `ember_knowledge` | Ember Knowledge | `GLOBAL_XP_BOOST` | +10% XP | 5 | Level 2 |
| `tempered_core` | Tempered Core | `BURN_RESISTANCE` | +4 points | 5 | Level 3 |
| `masterwork_focus` | Masterwork Focus | `MASTERY_XP_BOOST` | +12% | 5 | Level 4, `ember_knowledge` 2 |
| `artisan_slots` | Artisan Slots | `EXTRA_SLOT` | +1 slot | 3 | Level 6, `speed_forging` 2 |
| `blades_discipline` | Blades Discipline | `TRADE_SPEED` in `swords` | 4% faster | 4 | Level 8, `speed_forging` 3, `swords` mastery 8 |
| `bulwark_method` | Bulwark Method | `BURN_RESISTANCE` in `armors` | +3 points | 4 | Level 8, `tempered_core` 2, `armors` mastery 8 |
| `workshop_rhythm` | Workshop Rhythm | `TRADE_SPEED` in `tools` | 4% faster | 4 | Level 8, `speed_forging` 3, `tools` mastery 8 |
| `relic_attunement` | Relic Attunement | `MASTERY_XP_BOOST` in `artifacts` | +14% | 4 | Level 10, `masterwork_focus` 2, `artifacts` mastery 6 |

Four general perks, one slot perk, and four category perks that each need real investment in their
category first. That shape is worth keeping if you write your own.

## Effect types

| Type | `valuePerLevel` means | Applied as |
|---|---|---|
| `TRADE_SPEED` | Fraction faster (`0.05` is 5%) | Sum of levels, added to the mastery speed reduction |
| `BURN_RESISTANCE` | Percentage points of success chance | Sum of levels, added after the burn multiplier |
| `EXTRA_SLOT` | Slots | Unlocks that many locked slot icons, lowest first |
| `GLOBAL_XP_BOOST` | Fraction more XP | `xp x (1 + total)` |
| `MASTERY_XP_BOOST` | Fraction more mastery XP | `masteryXp x (1 + total)` |
| `COMMAND` | Nothing directly | Use `commands` (the effect is whatever you run) |

Effects are `valuePerLevel x currentLevel`, summed across every perk of that type the player holds.
`speed_forging` at 5 and `blades_discipline` at 4 give `0.25 + 0.16` = 41% faster sword crafts:
before mastery, and the combined reduction is floored at 90%.

## Defining one

```yaml
progression:
  perks:
    tempered_core:
      displayName: Tempered Core
      description:
        - '<gray>Improves success chance against burn.</gray>'
        - '<gray>Useful for risky and high-value crafts.</gray>'
      effectType: BURN_RESISTANCE
      valuePerLevel: 4.0
      maxLevel: 5
      baseCost: 1
      costPerLevel: 1
      requiredBlacksmithLevel: 3
      requiredPerkKey: null
      requiredPerkLevel: 0
      scopeKey: null
      requiredMasteryKey: null
      requiredMasteryLevel: 0
      commands: []
```

| Key | Meaning |
|---|---|
| `displayName` | Name in the menu; falls back to a prettified key |
| `description` | Lore lines |
| `effectType` | One of the six above |
| `valuePerLevel` | Interpreted by effect type |
| `maxLevel` | Levels available |
| `baseCost` | Cost of level 1 |
| `costPerLevel` | Added per level already held |
| `requiredBlacksmithLevel` | Global level needed to upgrade |
| `requiredPerkKey` / `requiredPerkLevel` | Prerequisite perk |
| `scopeKey` | Restrict the effect to one mastery category |
| `requiredMasteryKey` / `requiredMasteryLevel` | Mastery gate |
| `commands` | Console commands run on every upgrade |

## Cost

```
cost = max(1, baseCost + currentLevel x costPerLevel)
```

`baseCost: 1`, `costPerLevel: 1` gives 1, 2, 3, 4, 5: fifteen points for all five levels. A player
earning one point per level pays fifteen levels for one maxed perk.

## Buying

```
/blacksmith perks
```

Aliases `perk`, `tree`, `talents`. Click a node to upgrade it. The refusal is specific:

| Message key | Cause |
|---|---|
| `progressionPerkMaxed` | Already at `maxLevel` |
| `progressionPerkLocked` | Blacksmith level, prerequisite perk or mastery gate not met |
| `progressionPerkNoPoints` | Not enough perk points |
| `progressionPerkNotFound` | No such perk, or progression is disabled |

The purchase runs inside a database transaction under a per-player lock: points are deducted and the
level written together, or neither happens. Two rapid clicks cannot buy one level twice.

## Command perks

```yaml
prestige_title:
  displayName: Master Smith
  effectType: COMMAND
  valuePerLevel: 0
  maxLevel: 1
  baseCost: 10
  costPerLevel: 0
  requiredBlacksmithLevel: 50
  commands:
    - 'lp user {player} parent add mastersmith'
```

`COMMAND` perks have no built-in effect: the `commands` list is the whole point. Commands run from
console on every upgrade, so with `maxLevel` above 1 they run once per level.

<Callout type="warning" title="There is no refund and no respec">

Points spent are gone. Nothing in the plugin resets a perk tree: `/blacksmith admin perkpoints set`
grants points back, but it does not un-level the perk. If you plan to rebalance the tree, decide
before players spend.

</Callout>

## Reading perk state

```
%uxmblacksmith_perk_points%
%uxmblacksmith_perk_level_speed_forging%
%uxmblacksmith_perk_max_level_speed_forging%
%uxmblacksmith_perk_cost_speed_forging%
%uxmblacksmith_perk_value_speed_forging%
%uxmblacksmith_perk_next_value_speed_forging%
%uxmblacksmith_perk_name_speed_forging%
```

`perk_value_` formats by effect type: a percentage for `TRADE_SPEED`, points for
`BURN_RESISTANCE`, a whole number for `EXTRA_SLOT`.
