---
title: Reward files
order: 510
description: Every key of a reward, the item format, and shared rewards under rewards/.
icon: gift
---

A reward is written inside a crate's group, or once under `rewards/<id>.conf` and named with `from`. The keys are the
same in both places.

| Key | Default | Meaning |
|---|---|---|
| `weight` | the rarity's `weight` | A number; the chance is the weight over the group's sum |
| `display.name` | | The name in the preview and messages |
| `display.rarity` | none | A rarity of the crate |
| `display.lore` | `[]` | Lines under the preview's description |
| `display.icon` | the first item | `{ material, glow }`: the face in the preview |
| `display.order` | after the rest | The place on the preview page |
| `items` | `[]` | Item lines or item blocks |
| `commands` | `[]` | Console lines; `%player%`, `{player}`, `<player>` |
| `money` | `[]` | `"vault 500"`, `"exp 500"`, `"levels 5"` |
| `announce` | `false` | Broadcast whatever the threshold says |
| `permission` | none | Needed to win it |
| `limits.player` | none | `{ amount, reset }`: wins per player |
| `limits.global` | none | `{ amount, reset }`: wins across the server |
| `linked` | `[]` | Reward ids of the same crate given along with this one |
| `guarantee` | none | `{ mode = "once" or "every", opening = N }` |
| `fallback` | `false` | Given when the group has nothing else for the player |
| `luck` | `true` | `false` keeps rank luck off this reward |
| `from` | none | A file under `rewards/`. The crate's keys win over it |

`reset` is a span such as `12h`, `7d` or `30m`, counted from the win that reached the limit.

## The item format

An item is a line or a block:

```hocon
items = [
  "DIAMOND 3",
  {
    material = "NETHERITE_SWORD"
    amount   = 1
    name     = "<accent>The Legendary Sword"
    lore     = [ "<subtext>Sharpness V, and it keeps its edge." ]
    enchants { sharpness = 5, unbreaking = 3 }
    unbreakable = true
  }
]
```

| Key | Meaning |
|---|---|
| `material` | A material, or a [custom item](../integrations/custom-items.md) such as `oraxen:ruby` |
| `amount` | The stack size |
| `name`, `lore` | In the three shapes a name takes |
| `glow` | The glint |
| `model-data`, `item-model` | A resource pack's model |
| Anything else | Read by the uxmLib item reader: enchantments, `unbreakable`, trims, potions, a head's owner, a written book, damage |

A block that names only the first set is stored as a summary; one that names more is kept whole and built by the uxmLib
item reader, so an enchantment or a trim is never lost. A reward the editor makes from the item in your hand is
written the same way.

## Shared rewards

`rewards/legendary-sword.conf` ships as an example. A crate names it:

```hocon
sword { from = "legendary-sword", weight = 20, linked = ["scabbard"] }
```

Whatever the crate writes wins over the file. A shared reward cannot name another shared reward.

Related: [Rewards and groups](../features/rewards.md), [Crate files](crate-files.md)
