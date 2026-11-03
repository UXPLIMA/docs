---
title: Your first enchantment
order: 204
description: Write a sword enchantment that heals the wielder on a kill, give yourself the book, and apply it.
icon: sparkles
---

## 1. Write the file

Create `plugins/uxmEnchants/enchantments/vampire.conf`. The file name is the id, so the enchantment is `vampire` and its
node is `uxmenchants.enchantment.vampire`.

```hocon
name {
  en = "Vampire"
  tr = "Vampir"
}
description {
  en = "Heals you by <amount> hearts' worth on a kill."
  tr = "Bir öldürmede <amount> can kadar iyileştirir."
}

rarity    = rare
type      = normal
targets   = ["sword", "axe"]
max-level = 3

obtaining {
  table    = true
  villager = false
  loot     = true
  anvil    = true
}

trigger = kill

effects = [
  {
    effect = heal
    target = self
    amount { base = 2.0, per-level = 1.0, cap = 4.0, mode = add, curve = linear }
    actions = ["[sound] ENTITY_PLAYER_LEVELUP 0.6 1.6"]
  }
]
```

`<amount>` in the description is the heal effect's own `amount` at the level shown, so the sentence never drifts from
what the effect does.

## 2. Load it

```
/uxmenchants reload
```

A typo is refused with the file, the key and the line, and the old content stays loaded.

## 3. Try it

```
/uxmenchants give <you> vampire 3
```

Put the book and a sword in an anvil. The sword now reads `Vampire III` with the description under it. Kill a mob and
you are healed by four health and hear the sound. `/enchants info vampire` opens its page.

## 4. Let players find it

It is `rare`, so the table offers it from table level 10 at the rare `table-chance`, and loot chests may carry it.
Players need `uxmenchants.enchantment.vampire.table` to be offered it at the table; see
[Acquisition](../features/acquisition.md).

Related: [Enchantment files](../features/enchantment-files.md), [Effects](../features/effects.md)
