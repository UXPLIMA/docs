---
title: Numbers and descriptions
order: 403
description: Fixed numbers, modifier blocks, the three curves, and descriptions that name a level's numbers.
icon: calculator
---

Every number in an effect, and every chance in `rarities.conf`, is either fixed or a modifier block that scales with
the level.

```hocon
amount = 2.0
amount { base = 1.0, per-level = 0.5, cap = 4.0, mode = add, curve = linear }
```

| Key | Meaning |
|---|---|
| `base` | The value at level one |
| `per-level` | What each level above the first adds or multiplies |
| `cap` | The value it never goes above; leave it out for no cap |
| `mode` | `add`: `base + per-level * growth`; `multiply`: `base * (1 + per-level * growth)` |
| `curve` | How `growth` follows the level |

| Curve | growth | Level 1, 2, 3, 4, 5 |
|---|---|---|
| `linear` | `level - 1` | 0, 1, 2, 3, 4 |
| `diminishing` | The square root of `level - 1` | 0, 1, 1.41, 1.73, 2 |
| `exponential` | `2^(level - 1) - 1` | 0, 1, 3, 7, 15 |

There is no open maths expression in an effect, and there never will be: a number that only reveals itself when the
server runs it cannot be reviewed before it damages a server.

## Descriptions that name numbers

A description may name any number the enchantment works out at the level shown:

```
content.enchantments.blunted.description = "Trades an edge for a shove of <power> blocks."
```

`<power>` is the launch effect's own `power` at that level, the same number the effect uses, so the sentence cannot
drift the day somebody tunes it. In scope are every number argument of every effect by its own name, and `<level>`,
`<chance>`, `<cooldown>` and `<delay>`. When two effects carry the same name, the first wins. A tag naming a number the
enchantment does not have is left exactly as typed.

## The numbers block

For a number no effect carries, write it in `numbers`. Each line is an expression with `level` and every effect number
in scope:

```hocon
numbers {
  knockback-percent = "power * 100"
}
```

A line that cannot be read costs that one number and a line in the console, never the tooltip.

Related: [Effects](effects.md), [Display](display.md)
