---
title: Masteries
order: 208
description: Per-category levels that passively speed up crafts and cut burn.
icon: award
---

A mastery is a level in one category. Crafting from that category raises it; the level passively
improves every craft in it.

## The curve

```yaml
progression:
  masteryCurve:
    baseXp: 60
    growthFactor: 1.14
    maxLevel: 50
```

Same formula as the global level (`round(baseXp x growthFactor^(L-1))`) but cheaper and shorter:

| Mastery level | XP to next | Total XP to get there |
|---|---|---|
| 1 → 2 | 60 | 60 |
| 5 → 6 | 101 | 396 |
| 10 → 11 | 195 | 1,160 |
| 20 → 21 | 723 | 5,462 |
| 30 → 31 | 2,682 | 21,409 |
| 50 (max) | n/a | 299,671 |

Every player starts every mastery at level **1**, not 0. That matters for the bonus formulas below,
which subtract one.

## Which mastery a trade feeds

1. The trade's `masteryKey`, if set.
2. Otherwise the **category key** the trade was loaded from.

```yaml
item:
  masteryKey: "smithing"   # several categories can feed one shared track
```

A trade in a category with no key resolves to no mastery and grants global XP only.

## The bonuses

```yaml
progression:
  masteryBonuses:
    enabled: true
    tradeSpeedReductionPerLevel: 0.005
    maxTradeSpeedReduction: 0.20
    successChanceBonusPerLevel: 0.30
    maxSuccessChanceBonus: 12.0
```

Both are computed from `masteryLevel - 1` and then capped:

| Bonus | Per level | Cap | Cap reached at |
|---|---|---|---|
| Trade speed | 0.5% faster | 20% faster | Level 41 |
| Success chance | +0.30 points | +12 points | Level 41 |

At mastery 41 in `swords`, a 3-hour sword craft finishes in 2 hours 24 minutes and an 85% recipe
succeeds 97% of the time. That is the whole reward for specialising: there is no separate unlock.

Set `enabled: false` to keep masteries as a progress display with no mechanical effect.

## How it combines with everything else

Trade duration:

```
duration x= 1 - (TRADE_SPEED perk effects + mastery speed reduction)
```

floored at a 90% total reduction, and never below a hard minimum duration.

Success chance:

```
chance  = burn permission multiplier applied to successChance
chance += BURN_RESISTANCE perk effects
chance += mastery success bonus
clamped to 0–100
```

The permission multiplier **multiplies**; perks and mastery **add**. See
[Burn and risk](burn.md).

## Scoped perks

A perk with a `scopeKey` only counts inside that mastery. `blades_discipline` is `TRADE_SPEED` scoped
to `swords`, so its 4% per level applies to sword contracts and nothing else. That is how the perk
tree lets a player go deep rather than wide.

Perks can also *require* a mastery level to unlock: `requiredMasteryKey` and `requiredMasteryLevel`.
The four shipped category perks each need level 6–8 in their own category.

## Seeing it

```
/blacksmith mastery
```

Opens the mastery menu: one entry per category with the level, the XP bar, and both bonuses. Aliases
`/blacksmith masteries`.

In chat or a scoreboard:

```
%uxmblacksmith_mastery_level_swords%
%uxmblacksmith_mastery_percent_swords%
%uxmblacksmith_mastery_name_swords%
```

The suffix is the mastery key. `mastery_name_` returns the category's configured `name`, so a
scoreboard shows "Blades" rather than "swords".

In a trade's lore, `{player_mastery_level}`, `{mastery_name}`, `{mastery_speed_bonus}` and
`{mastery_success_bonus}` resolve against whichever mastery that trade feeds.

## Adjusting

```
/blacksmith admin mastery addxp <player> <mastery> <amount>
/blacksmith admin mastery setlevel <player> <mastery> <level>
```

<Callout type="tip" title="Name your category keys carefully">

The category key becomes the mastery key, the placeholder suffix and the perk `scopeKey`. It is the
one string you cannot rename later without resetting every player's mastery in it: the rows in
`player_masteries` are keyed by it.

</Callout>
