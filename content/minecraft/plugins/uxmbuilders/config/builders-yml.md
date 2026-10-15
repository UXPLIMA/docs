---
title: builders.yml
order: 303
description: Every builder, its levels, its NPC appearance and its unlock rules.
icon: hard-hat
---

Appearance and stats live together, one entry per builder. The file is validated when it loads and
refused with a numbered error if a rule is broken.

```yaml
builders:
  mason:
    display: "<gold>Mason</gold>"
    icon: PLAYER_HEAD
    description:
      - "<gray>A master of stone work.</gray>"
    appearance:
      type: PLAYER
      texture: ""
      signature: ""
      equipment:
        head: CHAINMAIL_HELMET
        hand: STONE_BRICKS
      trailParticle: ""
      placeSound: BLOCK_STONE_PLACE
    buildOrderOverride: ""
    unlock:
      price: 50000
      permission: ""
    levels:
      1: { name: "Stone Apprentice", blocksPerSecond: 5,  efficiency: 0.00, price: 0 }
      2: { name: "Journeyman Mason", blocksPerSecond: 8,  efficiency: 0.05, price: 75000 }
      3: { name: "Master Mason",     blocksPerSecond: 12, efficiency: 0.10, price: 150000 }
```

## The builder

| Key | Meaning |
|---|---|
| `display` | Name in menus, MiniMessage |
| `icon` | Material in the crew menu |
| `description` | Lore lines in the crew menu |
| `buildOrderOverride` | Forces a build order for this builder, above the package's |
| `unlock.price` | What buying the builder costs |
| `unlock.permission` | Needed to buy it; empty means anyone |

## appearance

| Key | Meaning |
|---|---|
| `type` | `PLAYER` or `MOB` |
| `texture`, `signature` | A skin for `PLAYER`; empty uses the default skin |
| `entity` | The mob for `MOB`, for example `ZOMBIE`; unknown values become a zombie |
| `equipment.head`, `equipment.hand` | What the NPC wears and holds while idle |
| `trailParticle` | A particle left behind as it walks |
| `placeSound` | Played as it places blocks |

While working, the NPC holds the block it is about to place.

## levels

| Key | Meaning |
|---|---|
| `name` | The level's title in menus and placeholders |
| `blocksPerSecond` | Placement and walking speed |
| `efficiency` | Chance, `0.00` to `1.00`, that a block costs no material |
| `price` | Cost of reaching this level; the first level's is ignored |

## Validation

| Code | Rule |
|---|---|
| `UB-060` | Levels start at 1 and increase without gaps; a builder has at least one |
| `UB-061` | `blocksPerSecond` never decreases from one level to the next |
| `UB-062` | `builders.default` in `config.yml` names a builder defined here |
| `UB-063` | A `MOB` appearance has an `entity` |

## Notes

- `uxmbuilders.builder.<id>` grants a builder and `uxmbuilders.builder.<id>.level.<n>` a level, as if
  bought. Both are registered so operators do not hold them by default.
- `/builders admin setbuilder <player> <builder> [level]` sets a level directly and saves it at once.

Related: [Builders](../features/builders.md), [packages.yml](packages-yml.md)
