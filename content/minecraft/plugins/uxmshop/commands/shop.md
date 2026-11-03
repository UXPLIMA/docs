---
title: Catalogue commands
order: 302
description: /shop, /sell and /value.
icon: shopping-cart
---

## Shop

| Command | Node | Does |
|---|---|---|
| `/shop` | `uxmshop.use` | Opens the [hub](../menus/catalogue.md) |
| `/shop open <section>` | `uxmshop.use`, and the section's `permission` when it names one | Opens one section |
| `/shop lang` | none | The language you read, and what the server has |
| `/shop lang set <language>` | none | Chooses your language |
| `/shop lang reset` | none | Follows the client again |
| `/shop lang set-player <player> <language>` | `uxmshop.admin.language` | Another player's language |
| `/shop lang reset-player <player>` | `uxmshop.admin.language` | Clears another player's choice |

## Sell

| Command | Node | Does |
|---|---|---|
| `/sell` | none | The same as `/sell hand` |
| `/sell hand` | `uxmshop.sell.hand` | Sells the stack in your hand |
| `/sell all` | `uxmshop.sell.all` | Sells everything the catalogue buys, up to `shop.sell-all-limit` items a call |
| `/sell window` | `uxmshop.sell.window` | Opens a window to drop items into, sold when it closes |

## Value

| Command | Node | Does |
|---|---|---|
| `/value` | `uxmshop.value` | What the item in your hand is worth, in both directions |

Every branch except `lang set-player` and `lang reset-player` is for players only.

Related: [Selling](../features/selling.md)
