---
title: Conditions and actions
order: 404
description: The three condition shapes, every action, and making an enchantment cost money to fire.
icon: git-branch
---

`conditions` and `actions` sit on an effect beside the chance. A condition decides whether the effect fires at all; an
action is what happens when it does. Both are asked on every firing, never at a level.

```hocon
{
  effect     = damage
  target     = victim
  amount     = 4.0
  conditions = ["money >= 250"]
  actions    = ["[take-money] 250", "[message] <body>The blade drinks."]
}
```

## Conditions

| Shape | Example | Reads |
|---|---|---|
| A comparison | `"%player_level% >= 10"` | Both sides through PlaceholderAPI when installed; a literal is itself. Operators `==`, `!=`, `>=`, `<=`, `>`, `<` |
| A balance | `"money >= 250"` | Whatever economy the server already has |
| An item count | `"item DIAMOND >= 2"` | That item in the player's inventory |

`%mcmmo_level_<skill>%` is read from mcMMO directly, without PlaceholderAPI. A server with no economy answers every
balance as zero, so a condition that asks for money refuses rather than passing.

## Actions

| Action | Example |
|---|---|
| `[message]` | `"[message] <body>The blade drinks."` |
| `[actionbar]` | `"[actionbar] <good>Struck"` |
| `[title]` | `"[title] <accent>Thunder"` |
| `[sound]` | `"[sound] ENTITY_PLAYER_LEVELUP 1 1"` |
| `[broadcast]` | `"[broadcast] <value><player><body> called the sky down"` |
| `[close]` | `"[close]"` |
| `[console]` | `"[console] say <player> struck"` |
| `[player]` | `"[player] spawn"` |
| `[take-money]` | `"[take-money] 250"` |
| `[take-item]` | `"[take-item] DIAMOND 2"` |

## An enchantment that costs money to fire

Write a condition that demands the balance and a `[take-money]` action that spends it. The plugin adds no currency of
its own and no key for one.

## Not drawn on the item

A condition can be true one second and false the next, so the display does not strike an enchantment through for an
unmet condition; only a missing node is drawn that way.

A line neither half can read refuses the whole reload and names the file and the key.

Related: [Effects](effects.md), [Integrations](../integrations/overview.md)
