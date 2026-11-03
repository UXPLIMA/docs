---
title: Reference
order: 701
description: Every %uxmenchants_...% placeholder, what it returns, and which need a player.
icon: list
---

Registered when PlaceholderAPI is installed. `/uxmenchants doctor` warns when it is not.

## About the held item

| Placeholder | Returns |
|---|---|
| `%uxmenchants_held_count%` | How many of ours the item in the main hand carries |
| `%uxmenchants_held_rarity_<rarity>%` | How many of that rarity it carries |
| `%uxmenchants_held_type_<type>%` | How many of that type it carries |
| `%uxmenchants_held_level_<id>%` | The level of that enchantment on it; `0` when it has none |
| `%uxmenchants_limit_total%` | The player's limit |
| `%uxmenchants_limit_used%` | What the held item uses of it |
| `%uxmenchants_limit_left%` | What is left |
| `%uxmenchants_charges%` | The charges left on the held item |
| `%uxmenchants_charges_max%` | The most charges an item holds |

## About the player

| Placeholder | Returns |
|---|---|
| `%uxmenchants_cooldown_<id>%` | Whole seconds left on that enchantment's cooldown; `0` when none runs |
| `%uxmenchants_descriptions%` | `true` or `false`: whether this player reads descriptions |

## About the server

These need no player, so a console line or a hologram can use them.

| Placeholder | Returns |
|---|---|
| `%uxmenchants_defined%` | How many enchantments this server defines |
| `%uxmenchants_defined_rarity_<rarity>%` | How many of that rarity |
| `%uxmenchants_defined_type_<type>%` | How many of that type |

Every other placeholder needs an online player and returns nothing without one.

```
Sword: %uxmenchants_held_count%/%uxmenchants_limit_total% enchantments
Vein Miner: %uxmenchants_held_level_vein-miner%
```

Related: [Applying](../features/applying.md#the-limit), [Charges and cooldowns](../features/charges-and-cooldowns.md)
