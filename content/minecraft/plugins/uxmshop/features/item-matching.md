---
title: Item matching
order: 402
description: The four match modes and the fields that may be ignored.
icon: scan
---

`match` is a value on each line, because one comparison cannot be right for cobblestone and for a named sword at once.

| `match` | Takes |
|---|---|
| `material` | Anything of that material, whatever has been done to it |
| `material-and-name` | The material and the display name |
| `full-meta` | Every field |
| `full-meta-ignoring:damage,lore` | Every field except the ones named |

The fields that may be ignored are `name`, `lore`, `enchants`, `stored-enchants`, `custom-model-data`, `damage`,
`unbreakable`, `potion`, `potion-effects`, `colour`, `firework`, `trim`, `head`, `spawner`, `patterns` and `model`. Use
the last form for tools, so a used pickaxe sells as a pickaxe.

When two lines accept the same item, the stricter one wins, so the answer never depends on file order. A player shop
offer takes the same modes in `/myshop add`.

Related: [Sections and lines](sections-and-lines.md)
