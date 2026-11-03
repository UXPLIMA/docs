---
title: Applying
order: 305
description: The one rule every route follows, the refusals, the limit and its nodes, and removed enchantments.
icon: anvil
---

The table, the anvil, a clicked book, loot and `/uxmenchants apply` all pass through one rule, which is the only way
the limit and the conflicts can be true everywhere at once.

## When an enchantment is refused

| Refusal | Message key |
|---|---|
| The player lacks the enchantment's node (table, anvil, clicked book and `apply` only) | `enchant.not-permitted` |
| The item is not in `table-items` (table) or `anvil-items` (every other route) | `enchant.wrong-item` |
| The level is above `max-level`, or `overpower-max` on an overpowering anvil | `enchant.over-maximum` |
| It conflicts with something on the item, from either side, ours or another plugin's | `enchant.conflicts` |
| It shares an exclusive group with something on the item | `enchant.conflicts` |
| A `required` enchantment is missing | `enchant.requires` |
| The item is at its limit and this would be a new enchantment | `enchant.limit-reached` |
| Its type is at that type's `limit-per-item` | `enchant.type-limit-reached` |

A villager trade and a loot chest have nobody standing there, so they ask no node.

`removed` enchantments come off first, so an enchantment that grows out of another replaces it even on a full item.
Raising the level of one already on the item is always allowed by the limit.

## The limit

```hocon
limit {
  base              = 3
  node-prefix       = "uxmenchants.limit."
  maximum-from-node = 32
}
```

Every player may carry `base` of ours on one item. A node `uxmenchants.limit.<number>` raises it to that number; the
highest node a player holds wins, and nodes above `maximum-from-node` are not read, so a typo cannot uncap it. A node at
or below the base changes nothing. Vanilla enchantments never count. A per type limit is separate and lives in
`types.conf`.

`/enchants limit` prints the base, the node, what the held item uses and what is left; `%uxmenchants_limit_total%`,
`_used%` and `_left%` give the same numbers.

## The node of an enchantment

`uxmenchants.enchantment.<id>`, or the file's `permission`. Everyone has the shipped twelve by default. A player who
lacks it cannot be given it at the table or the anvil, and an item that already carries it does not fire for them and
draws the line struck through.

Related: [Acquisition](acquisition.md), [Permissions](../permissions/reference.md)
