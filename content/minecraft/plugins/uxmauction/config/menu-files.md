---
title: Menu files
order: 505
description: The keys of a window file, the tile line, the item material form, and every action the windows know.
icon: layout-template
---

Every window in `menus/` is a file: move a slot, change a material, take a button out, and the plugin draws what you
wrote.

| Key | Meaning |
|---|---|
| `title` | `@menu.<window>.title`, or your own words |
| `rows` | One to six |
| `slot`, `slots` | Where an item sits |
| `priority` | Which item wins a slot two of them ask for |
| `type` | `previous` or `next` |
| `click` | `left`, `right`, `shift_left`, `shift_right` |
| `list` | `source` and `template` for the squares a list fills |

A list template's `material = "auction:item:CHEST"` draws the real item of the row, with `CHEST` when there is none.
`lore = ["tile:<colour> @<block> <facts>"]` draws the house tooltip.

## Actions

| Action | Does |
|---|---|
| `auction:select` | Opens the listing of the clicked row |
| `auction:house`, `auction:home` | The house; `home` with nothing looked for |
| `auction:tabs` | The categories |
| `auction:category:<name>`, `auction:category:all` | The house on one tab, or all |
| `auction:sort:<name>`, `sort:next`, `sort:previous` | `newest`, `oldest`, `price-low`, `price-high`, `ending-soon` |
| `auction:search` | Closes the window and waits for words in chat |
| `auction:mailbox`, `auction:claim` | The mailbox; takes the clicked row |
| `auction:mine:listings`, `mine:bids`, `mine:orders` | The three tabs |
| `auction:cancel` | Takes the clicked listing back, or gives an order's money back |
| `auction:buy:all`, `buy:one`, `buy:<number>` | Buys from the open listing |
| `auction:bid` | Bids the next step |
| `auction:type:price`, `type:units`, `type:time` | Asks for the value in chat |
| `auction:price:<number>` | Adds to the price; a minus sign takes off |
| `auction:units:<number>` | More or fewer |
| `auction:time:<length>` | Stands for that long |
| `auction:currency [id]` | That currency, or the next |
| `auction:kind:fixed`, `kind:auction` | Sell window: the kind |
| `auction:retail` | Sell window: in parts or one lot |
| `auction:list` | Sell window: puts it on the shelf |
| `auction:match:material`, `enchanted`, `exact` | Order window: the match |
| `auction:auto` | Order window: fills by itself or waits |
| `auction:ask` | Order window: opens the order |
| `auction:watch:material`, `enchanted`, `exact`; `auction:unwatch` | Watch window |
| `auction:restore` | Admin log: gives the item of the clicked line back |

List sources: `auction:shelf`, `auction:categories`, `auction:selected`, `auction:selling`, `auction:wanted`,
`auction:mine`, `auction:bids`, `auction:orders`, `auction:mailbox`, `auction:watches`, `auction:history`, `auction:log`.

Every window also knows `close`, `open:<menu>`, `command:<line>`, `message:<line>` and
`sound:<name> <volume> <pitch>`.

Related: [Menus](../menus/overview.md), [Messages](messages.md)
