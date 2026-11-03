---
title: Selling
order: 305
description: /sell hand, all and window, /value, and the blacklist.
icon: hand-coins
---

| Command | Does |
|---|---|
| `/sell`, `/sell hand` | The stack in your hand |
| `/sell all` | Everything the catalogue buys, up to `shop.sell-all-limit` (2304) items a call; run it again for more |
| `/sell window` | A [window](../menus/catalogue.md#sell) to drop items into; what cannot be sold comes back, then the rest is sold |
| `/value` | What the item in your hand is worth, in both directions |

Every path goes through the same till, so a line permission, a condition, a quota and a band still hold. A sale of a
whole inventory is many trades, and the log records it as many.

## Blacklist

```hocon
blacklist {
  materials = []
  bypass    = "uxmshop.bypass.blacklist"
}
```

Materials nobody may trade in either half. A holder of `uxmshop.bypass.blacklist.<material>` is past it for that
material.

Related: [Catalogue commands](../commands/shop.md#sell)
