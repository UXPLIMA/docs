---
title: Abilities
order: 404
description: The keys of an ability file, points, and the twenty eight shipped abilities.
icon: sparkles
---

An ability file is tuned here; its behaviour (`kind`) is written in the plugin. A skill lists the abilities it opens,
so moving an ability between skills is two lines in two files.

| Key | Meaning |
|---|---|
| `kind` | Which built-in behaviour it is |
| `unlock-level` | The skill level it opens at |
| `level-interval` | Skill levels between ability levels |
| `maximum-level` | The highest ability level |
| `base-value`, `value-per-level` | What the first level gives, and each level after |
| `level-formula`, `value-formula` | Formulas in their place |
| `conflicts` | Abilities it cannot be held with; the weaker is suppressed |
| `chance` | For an ability that rolls |
| `cooldown-ms` | For an ability that acts in a burst |
| `cost` | An ability's own charge, such as `cost { currency = "exp", money = 25 }`, or a resource |
| `point-cost`, `point-cost-per-level` | What a level costs when points are on |

## Points

```hocon
points { enabled = false, per-skill-level = 1, starting = 0, refundable = true }
```

Off, every ability opens itself at its level. On, levelling a skill pays points into that skill's line, and the player
spends them with `/skills buy <ability>`; `/skills refund <skill>` takes them back when `refundable`. A fraction of
`per-skill-level` is rounded down. Points are not mana: nothing casts anything with them. It ships off so an upgrading
server does not find every player's abilities taken away.

## The shipped abilities

Value is base plus per level.

| Ability | Skill | Kind | Opens at | Every | Levels | Value | Does |
|---|---|---|---|---|---|---|---|
| `vein-miner` | mining | `vein-miner` | 20 | 20 | 5 | 3 + 2, cooldown 8 s | Breaking one block of an ore body breaks the ore joined to it; the value is how many |
| `double-drop` | mining | `double-drop` | 5 | 8 | 10 | 0.05 + 0.02 | A chance for a broken block to drop what it dropped, twice |
| `tool-saving` | mining | `tool-saving` | 10 | 10 | 8 | 0.04 + 0.02 | A chance for a broken block to cost the pickaxe no durability |
| `wide-dig` | excavation | `wide-dig` | 25 | 25 | 3 | 9 + 0, cooldown 6 s | Digging one soft block digs the square around it |
| `soil-yield` | excavation | `double-drop` | 5 | 8 | 10 | 0.05 + 0.02 | A chance for dug ground to give up a second drop |
| `tree-feller` | woodcutting | `tree-feller` | 20 | 20 | 5 | 12 + 6, cooldown 10 s | Felling one log fells the tree above it; the value is how many logs |
| `timber-yield` | woodcutting | `double-drop` | 5 | 8 | 10 | 0.05 + 0.02 | A chance for a felled log to give up a second one |
| `auto-replant` | farming | `auto-replant` | 10 | 15 | 5 | 0.3 + 0.15 | A harvested crop is planted again from what it dropped |
| `herd-master` | farming | `herd-master` | 20 | 20 | 5 | 0.05 + 0.03 | A chance for a bred pair to leave a second young |
| `harvest-yield` | farming | `double-drop` | 5 | 8 | 10 | 0.05 + 0.02 | A chance for a mature crop to give up a second harvest |
| `treasure-hunter` | fishing | `treasure-hunter` | 10 | 12 | 8 | 0.04 + 0.02 | A chance for the rod to pull up an extra item |
| `quick-bite` | fishing | `quick-bite` | 15 | 15 | 6 | 0.05 + 0.03 | The wait for a bite is shorter, as a share taken off it |
| `critical-strike` | combat | `critical-strike` | 5 | 10 | 8 | 1.3 + 0.1, chance 0.1 | A chance for a melee hit to carry extra damage, as a multiplier |
| `bleed` | combat | `bleed` | 15 | 15 | 6 | 1.0 + 0.5, chance 0.15 | A melee hit leaves a wound that keeps taking damage |
| `life-steal` | combat | `life-steal` | 25 | 20 | 5 | 1.0 + 0.5 | A melee kill gives some health back |
| `pierce` | archery | `pierce` | 5 | 10 | 8 | 1.15 + 0.05 | A hit at range carries extra damage, as a multiplier |
| `arrow-retrieval` | archery | `arrow-retrieval` | 15 | 12 | 8 | 0.1 + 0.05 | A chance to get the arrow you fired back |
| `daze` | archery | `daze` | 30 | 20 | 4 | 0.08 + 0.04 | A chance for a hit at range to stun what it hits |
| `thick-skin` | defence | `thick-skin` | 5 | 10 | 8 | 0.02 + 0.01 | Damage taken is reduced by a share of itself |
| `shield-wall` | defence | `shield-wall` | 20 | 15 | 6 | 0.05 + 0.03 | A raised shield stops more than it would have |
| `second-wind` | defence | `second-wind` | 30 | 20 | 4 | 2.0 + 1.0, cooldown 120 s | Health below the threshold comes back over the duration |
| `swift` | agility | `swift` | 5 | 10 | 8 | 0.02 + 0.01 | Sprinting is faster, as a share added to the walk speed |
| `soft-landing` | agility | `soft-landing` | 15 | 15 | 6 | 0.1 + 0.05 | A fall costs less, as a share of the damage taken off |
| `deep-lungs` | agility | `deep-lungs` | 25 | 20 | 5 | 0.15 + 0.1 | Breath under water lasts longer |
| `potion-master` | alchemy | `potion-master` | 10 | 12 | 8 | 0.05 + 0.03 | A brewed potion lasts longer |
| `frugal-brewer` | alchemy | `frugal-brewer` | 20 | 15 | 6 | 0.05 + 0.03 | A chance for the stand to keep its ingredient |
| `arcane-saving` | enchanting | `arcane-saving` | 10 | 12 | 8 | 0.03 + 0.02 | An enchanting table costs fewer levels |
| `anvil-saving` | enchanting | `anvil-saving` | 15 | 12 | 8 | 0.03 + 0.02 | An anvil costs fewer levels |

Every shipped ability costs one point and one more per level. `soil-yield`, `timber-yield` and `harvest-yield` are the
`double-drop` kind tuned for other blocks.

Related: [Ability files](../config/content-files.md#abilities)
