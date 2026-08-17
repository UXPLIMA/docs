---
title: Progression
order: 207
description: The blacksmith level, the XP formula, and the reward policy you can tune.
icon: trending-up
---

Every completed trade grants **global XP** and **mastery XP**. Global XP raises the blacksmith level;
each level grants perk points. All of it lives under `progression` in `config.yml`.

```yaml
progression:
  enabled: true
  levelCurve:
    baseXp: 100
    growthFactor: 1.18
    maxLevel: 100
  masteryCurve:
    baseXp: 60
    growthFactor: 1.14
    maxLevel: 50
```

Set `enabled: false` and the whole system (levels, masteries, perks, the two menus, `reqLevel`
gates and the vanilla crafting gate) goes inert.

## The curve

XP required to leave level *L* is:

```
round(baseXp x growthFactor^(L-1))
```

With the shipped global curve:

| Level | XP to next | Total XP to get there |
|---|---|---|
| 1 → 2 | 100 | 100 |
| 2 → 3 | 118 | 218 |
| 5 → 6 | 194 | 715 |
| 10 → 11 | 444 | 2,353 |
| 20 → 21 | 2,321 | 14,663 |
| 30 → 31 | 12,150 | 79,095 |
| 50 → 51 | 332,827 | 2,181,311 |

At level `maxLevel` the requirement is 0, XP stops accumulating on the level bar, and `totalXp` keeps
counting. `growthFactor: 1.18` is steep: level 50 costs roughly 3,300 times what level 1 did, and
reaching 100 is not a realistic goal on the default curve. Lower it to about 1.10 if you want most of
the server past level 30.

## What a trade is worth

If the trade sets `xpReward`, that number is used. Otherwise the default policy computes it:

```
xp  = defaultTradeXp
xp += (time ÷ 60) x timeXpPerMinute
xp += requirementCount x requirementXpPerRequirement
xp += totalRequiredAmount ÷ requirementAmountDivisor
if 0 ≤ successChance < 100:
    xp x= 1 + ((100 - successChance) ÷ 100) x riskBonusMultiplier
if asCommand:
    xp x= 1.10
```

Then, in order:

```
if instantCraft:  xp x= instantCraftMultiplier
if burned:        xp x= burnedTradeXpMultiplier
xp x= 1 + (total GLOBAL_XP_BOOST perk effect)
```

| Setting | Default | Effect |
|---|---|---|
| `defaultTradeXp` | `20` | The floor for any trade |
| `timeXpPerMinute` | `5.0` | Longer crafts pay more |
| `requirementXpPerRequirement` | `4.0` | More distinct requirements pay more |
| `requirementAmountDivisor` | `16.0` | Bulk costs pay more, divided by this |
| `riskBonusMultiplier` | `0.35` | A 70%-chance trade earns 1.105x |
| `instantCraftMultiplier` | `0.65` | Instant crafts are worth less |
| `burnedTradeXpMultiplier` | `0.10` | A burned trade still pays a tenth |
| `perkPointsPerLevel` | `1` | Perk points per level up |

The `asCommand` 1.10x is a fixed constant, not a config value.

Worked example: the shipped `Netherite Warlord Blade`: `time: 420`, four requirements totalling 22
items, `successChance: 70`.

```
20 + (420÷60 x 5) + (4 x 4) + (22÷16)   = 20 + 35 + 16 + 1.375 = 72.375
x 1 + (30÷100 x 0.35)                    = x 1.105 = 79.97  →  80 XP
```

Its config sets `xpReward: 155` instead, roughly double what the formula would give, which is the
point of `xpReward`: use it when a trade should be worth more than its shape suggests.

## Mastery XP

If the trade sets `masteryXpReward`, that number is used. Otherwise:

```
masteryXp = max(defaultMasteryXp, resolvedGlobalXp x masteryFromGlobalXpMultiplier)
```

then the same `instantCraft`, `burned` and perk multipliers, with
`burnedMasteryXpMultiplier` and the `MASTERY_XP_BOOST` perk effect.

| Setting | Default |
|---|---|
| `defaultMasteryXp` | `12` |
| `masteryFromGlobalXpMultiplier` | `0.60` |
| `burnedMasteryXpMultiplier` | `0.25` |

Mastery keeps a **quarter** of its XP on a burn, against a tenth for global XP. That gap is
deliberate: a run of bad luck still moves you toward burning less.

## Which mastery a trade feeds

1. The trade's `masteryKey`, if set.
2. Otherwise the category key the trade was loaded from.

See [Masteries](masteries.md).

## Level ups

XP is applied, then levels are consumed in a loop, so a single large grant can raise several levels
at once and grant a perk point for each. At `maxLevel` the bar is pinned to 0 and `totalXp` keeps
rising: nothing is lost, it just stops converting.

## Duplicate protection

Each grant is written to `progression_events` keyed by a source id. If the same trade completion is
processed twice (a race, a retry after a crash) the second write is recognised as a duplicate and
no XP is granted. You will not see doubled XP from a restart mid-completion.

## Inspecting and adjusting

```
/blacksmith admin profile <player>
/blacksmith admin xp add <player> <amount>
/blacksmith admin level set <player> <level>
/blacksmith admin perkpoints <add|set> <player> <amount>
```

All four need `uxmblacksmith.admin`. See [Commands](../commands.md).

<Callout type="tip" title="Set the curve before launch, not after">

Changing `growthFactor` later re-prices every level for players who already have them. Their stored
level does not move, but the next level suddenly costs a different amount, which reads as a nerf
even when it is a buff. Decide the curve while the server is empty.

</Callout>
