---
title: Tools and skins
order: 310
description: Tool wear, enchantments that drive yield, speed and wear, and suits as rank perks.
icon: shirt
---

## Tools

`tool.conf`:

```hocon
tool {
  durability    = false
  when-worn-out = "park"
  enchantments {
    enabled = false
    yield { enchantment = "fortune", extra-per-level = 1.0, chance = 0.35 }
    speed { enchantment = "efficiency", per-level = 0.05, floor = 0.5 }
    wear  { enchantment = "unbreaking", skip-chance-per-level = 0.25 }
  }
}
```

With `durability` on, the held tool wears while the minion works: a running cost that cannot be paid away with cash.
`when-worn-out = "break"` destroys it and stops the minion until it gets another; `"park"` stops it at one durability,
losing nothing. The anvil on the panel gives a fresh tool.

A minion holds whatever a player puts in its tool slot, and the tool that comes out is the tool that went in. With
`enchantments.enabled`, you name which enchantment drives which effect, ours, another plugin's or vanilla
(`fortune` and `minecraft:fortune` are the same):

| Effect | Keys | Does |
|---|---|---|
| `yield` | `enchantment`, `extra-per-level`, `chance` | A roll per action for more output: `extra-per-level` items per level, rounded down |
| `speed` | `enchantment`, `per-level`, `floor` | Takes `per-level` off the interval per level, never below `floor` of it |
| `wear` | `enchantment`, `skip-chance-per-level` | A chance per level to skip the durability cost |

## Skins

`skins.conf`. A skin changes nothing a minion does, which is what makes a suit sold as a rank perk safe.

| Key | Meaning |
|---|---|
| `name` | Per language, or `@content...` |
| `head` | `owner` (whoever placed it), an account name, `hdb:<id>`, or a base64 texture |
| `body`, `legs`, `boots` | `{ red, green, blue }`, 0 to 255 |
| `held-item` | What it is drawn holding; empty for the level's own |
| `permission` | Who may wear it; empty for everybody |

| Skin | Node |
|---|---|
| `default` | none |
| `ember`, `tide`, `grove`, `dusk` | `uxmminions.skin.<name>` |

A skin the owner chose overrides the level's look and is kept through every upgrade. The [skin window](../menus/skin.md)
draws a locked skin rather than hiding it: a perk nobody can see is a perk nobody buys.

Related: [tool.conf](../config/tool-conf.md), [skins.conf](../config/skins-conf.md)
