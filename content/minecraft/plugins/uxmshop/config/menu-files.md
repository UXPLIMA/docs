---
title: Menu files
order: 605
description: The keys of a window file, tiles, sources, conditions and actions.
icon: layout-template
---

| Key | Meaning |
|---|---|
| `title` | `@menu.<window>.title` |
| `rows` | One to six |
| `slot`, `slots` | Where an item sits; a range is `"0-8"` |
| `priority` | Which item wins a shared slot; the highest is drawn last |
| `material` | A material, or a `%value%` the plugin fills |
| `name` | A button's key with `@`, or a single space on a tile |
| `lore` | One tile per line: `tile:<colour> @<block> <facts>` |
| `type` | `previous` or `next` |
| `click` | `left`, `right`, `shift_left`, `shift_right` |
| `source` | Which list fills the slots |
| `view` | Who sees the item, as conditions |
| `chest-only` | Keep a Bedrock player on the chest window |

| Name | Kind | Window |
|---|---|---|
| `shop:sections`, `shop:lines` | Source | Hub, section |
| `shop:all-shops`, `shop:offers` | Source | Shops, a shop |
| `shop:owner`, `shop:staff` | Condition | A shop |
| `shop:open`, `shop:hub`, `shop:buy`, `shop:sell` | Action | Hub, section |
| `shop:more:<n>`, `shop:less:<n>`, `shop:confirm`, `shop:offer-confirm`, `shop:back` | Action | Quantity |
| `shop:visit` | Action | Shops |
| `shop:offer-buy`, `shop:offer-sell`, `shop:restock:<n>`, `shop:withdraw:<n>`, `shop:drop-offer`, `shop:toggle-sells`, `shop:toggle-buys`, `shop:toggle-tells` | Action | A shop |

Every window also knows `close`, `open:<menu>`, `command:<line>`, `message:<line>` and
`sound:<name> <volume> <pitch>`. `/shopedit tiles`, `move` and `icon` change tiles in game.

Related: [Menus](../menus/overview.md), [Messages](messages.md)
