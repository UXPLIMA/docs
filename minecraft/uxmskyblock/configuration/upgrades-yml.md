---
title: upgrades.yml
order: 760
---

`upgrades.yml` defines every upgrade, its tiers, and what each tier costs. Add,
remove, or retune tiers freely — the [Upgrades menu](../menus/upgrades.md) is built
from this file automatically.

For the default tier tables in friendly form, see [Upgrades](../progression/upgrades.md)
and [Generators](../progression/generators.md).

---

## Two Upgrade Types

| `type` | Meaning |
|--------|---------|
| `value` | The level holds a single number — a limit, multiplier, or size |
| `generator` | The level holds weighted block `chances` |

---

## A `value` Upgrade

```yaml
upgrades:
  team-limit:
    display-name: "&eTeam Limit"
    icon: PLAYER_HEAD
    slot: 10
    type: value
    levels:
      1: { value: 4,  required-level: 0,  required-money: 0 }
      2: { value: 6,  required-level: 5,  required-money: 5000 }
      3: { value: 8,  required-level: 12, required-money: 15000 }
      4: { value: 12, required-level: 25, required-money: 40000 }
```

| Field | Meaning |
|-------|---------|
| `display-name` | Item name in the menu |
| `icon` | Item material |
| `slot` | Menu slot |
| `type` | `value` |
| `levels.<n>.value` | The number this tier sets |
| `required-level` | Island level needed |
| `required-money` | Vault cost (ignored when economy is off) |

What `value` *means* depends on the upgrade:

| Upgrade | `value` is... |
|---------|---------------|
| `team-limit` | Max members |
| `size` | Island size in blocks (protection radius = value/2) |
| `crop-growth` | % chance to double a crop |
| `mob-drops` | Drop multiplier |
| `spawner-rates` | Spawner delay multiplier (lower = faster) |

---

## A `generator` Upgrade

```yaml
generator:
  display-name: "&7Generator"
  icon: COBBLESTONE
  slot: 24
  type: generator
  levels:
    1:
      required-level: 0
      required-money: 0
      chances:
        COBBLESTONE: 100
    2:
      required-level: 6
      required-money: 8000
      chances:
        COBBLESTONE: 80
        COAL_ORE: 15
        IRON_ORE: 5
```

`chances` are **weights** — a block is rolled in proportion to its weight when the
generator forms. See [Generators](../progression/generators.md).

---

## Adding Your Own Tier

Just append a new numbered level:

```yaml
    5: { value: 16, required-level: 60, required-money: 200000 }
```

The menu picks it up automatically. Run `/is admin reload` to apply.
